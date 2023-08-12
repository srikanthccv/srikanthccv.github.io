Title: Threading and forking
Date: 2023-08-12
Category: poasts
Tags: os, threads, fork, linux

Occasionally, you encounter an issue that teaches or makes you revisit the fundamentals. This is one such issue which is worth sharing. Until recently, I was heavily involved in a Python project that helps developers to instrument their code and troubleshoot performance issues. The primary audience[^1] of this library writes web application services, and the vast majority of those applications are hosted with a WSGI server such as Gunicorn or uWSGI, either directly or behind a lightweight web server such as nginx. We have received several reports from users that the library does not work when used with Gunicorn. The issue was that their telemetry data was not getting exported. I set out to investigate the issue and little did I know that I would (re)-learn a lot about the Linux system call `fork`.

The landing page of [https://gunicorn.org/](https://gunicorn.org/) says "It's a pre-fork worker model". The preforking model has a long history and dates back to the early days of Apache. The idea is to create a pool of worker processes to handle incoming requests. Why is there a need to create a pool of worker processes, you ask? The multi-threading is not truly parallel Python because of the Global Interpreter Lock (GIL). The GIL is a mutex that protects access to Python objects, preventing multiple threads from executing Python bytecodes at once. This means that Python threads are unable to fully utilize multiple cores. The preforking model is a way to get around this limitation and utilize multiple cores.

The library in question uses several background threads to do batch processing. These background threads use synchronization primitives like Lock, Event, and Condition variables to export data periodically when the queue is full or when a timeout occurs. It's when you mix the threading, and forking that things get weird. 

The [`fork`](https://man7.org/linux/man-pages/man2/fork.2.html) manual page says 

>The child process is created with a single thread—the one that called fork(). The entire virtual address space of the parent is replicated in the child, including the states of mutexes, condition variables, and other pthreads objects. 

This means when a multi-threaded program forks, the child process has only one thread, and the state of synchronization primitives is copied. The lock acquired by the parent process is also acquired by the child process. This is a problem because the child process is unaware of the parent process, and the lock is never released and deadlocks the child process.

So what is the solution? 

- One way to address this problem is to declare your package is not fork-safe and ask the users to do whatever is necessary to make it work[^2].

- Or can the library be made fork-safe?

I got curious mainly because Python stdlib itself could have some code that is not fork-safe, and it must have a mechanism to handle this situation. It's time to look at the issue tracker to see the past discussions. There are several issues that are related to this problem. Following are some of the issues that are relevant to this discussion which I read through:

- [https://github.com/python/cpython/issues/32958](https://github.com/python/cpython/issues/32958)
- [https://github.com/python/cpython/issues/50970](https://github.com/python/cpython/issues/50970)
- [https://github.com/python/cpython/issues/51172](https://github.com/python/cpython/issues/51172)
- [https://github.com/python/cpython/issues/60704](https://github.com/python/cpython/issues/60704)
- [https://github.com/python/cpython/issues/74014](https://github.com/python/cpython/issues/74014)
- [https://github.com/python/cpython/issues/74580](https://github.com/python/cpython/issues/74580)

And quickly skimming through the PRs that are linked, I found the `register_at_fork` from the `os` module is used to sanitize the locks and other synchronization primitives. My immediate thought was, The man page for fork had mentioned a related function which I didn't bother to read carefully. The `pthread_atfork` that is used to register handlers to be called when a new process is created with `fork`. 

The NOTES section says

>When fork(2) is called in a multithreaded process, only the
calling thread is duplicated in the child process.  The original
intention of pthread_atfork() was to allow the child process to
be returned to a consistent state.  For example, at the time of
the call to fork(2), other threads may have locked mutexes that
are visible in the user-space memory duplicated in the child.
Such mutexes would never be unlocked, since the threads that
placed the locks are not duplicated in the child.  The intent of
pthread_atfork() was to provide a mechanism whereby the
application (or a library) could ensure that mutexes and other
process and thread state would be restored to a consistent state.

Ah yes, this is exactly what we need. And I sent a PR to use `register_at_fork` to reinitialize the possibly corrupted state in the child process. The PR was merged, and users were happy to see their distributed tracing data again. A set of users (who used uWSGI) were still facing the issue. After a few days of debugging, I found that the hook is never invoked because the uWSGI forks outside the Python interpreter. The `fork` is called from the C code, and the `register_at_fork` is not invoked. Well, that's unfortunate. It didn't occur to me immediately that the [comment](https://github.com/python/cpython/issues/50970#issuecomment-1093477842) from the @pitrou can also be used  i.e track the pid and reinitialize when the pid changes. I sent another PR to do exactly that and it now works everywhere.

Oof, that was a lot of work and fun at the same time.


[^1]: Arguably, the primary audience.

[^2]: There are server hooks that can be used to work around this problem but that wouldn't make the library "auto-instrumentation".