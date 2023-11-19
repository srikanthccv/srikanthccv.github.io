Title: Making changes to a large C++ project 
Date: 2023-11-19
Category: posts
Tags: programming, clickhouse, entries

I recently worked on adding a [DDSketch](https://github.com/ClickHouse/ClickHouse/pull/56342) support in ClickHouse. ClickHouse codebase is large; larger than any codebase I've worked with before. It's written in C++, a language I haven't used since college. My experience with C++ was limited to writing small programs for timed programming contests. Having to work with a large codebase in a language with little to no experience was a challenge because there was no buddy pair to guide me through. I had to learn how to navigate the unknown codebase, find the right place to make changes, test my changes, and fix the build issues. This post is for my future self, to document what worked and remind myself you can hack your way through almost anything.


C++ is a different beast, unlike my primary choice of programming language Python/Go/JavaScript, where you usually can get the application to work with a simple run command. ClickHouse uses CMake as its build system. I didn't bother to read about it before I started working on the feature. I just jumped in and started making changes to the codebase. It turned out to be a terrible way to go about it. The seemingly simple `cmake --build build .` command failed with a very long cryptic error message. The Google search didn't turn up anything useful; The tokens in the error message are several orders of magnitude more than the maximum number of tokens allowed in ChatGPT. 


thak gaya hu bro (╯°□°)╯︵ ┻━┻. 


After countless failed attempts with (mostly) random changes over the next few days, a tweaked Google search led me to a mariadb's Jira ticket with a related crc32 error originating from rocksdb. The library linking issue was fixed after editing relevant CMakeLists.txt files. Next, tens of compilation errors spewed out to the console because ClickHouse build flags are very strict. ChatGPT shines here - you can paste the error, and it tells you how to address it in modern C++. It was like having an experienced C++ developer to help you on demand at a keyboard away. 


I was a little excited when it finally got compiled. To no surprise, it started crashing at the boot stage. It was another nightmare issue to troubleshoot. I wrote about my debugging philosophy in another [writeup]({filename}./dont-use-debugger.md). The techniques were ineffective given my expertise here is none. It was time to fire up the professional debugger like a "real" developer. The debugger came to the rescue and helped me track down the issue. The reason behind the crash was any Exception thrown would never get caught in the `try...catch` block and terminate the server.


This time again, a carefully crafted Google search led me to a JetBrains discussion forum. It gave me some pointers to dig further. I came out of this mess cursing the Apple linker. The generated build config used the default linker on MacOS. I don't know the underlying issue, but using the llvm clang for compilation and Apple linker crashes any program. Setting the `LLD_PATH` with llvm linker (`/opt/homebrew/opt/llvm/bin/ld64.lld`) for Mac finally produced a build that doesn't crash. I have two related posts coming up next; explaining _DDSketch for dummies_ and specific details about _writing quantile aggregation function in ClickHouse_.

Notes for myself

- I should get familiar with the build systems and tooling before jumping into making any changes.
- Use a debugger when learning a new language to look at the frame variables, stack traces, control flow etc. It's the only time when debugger use is justified.
- Do not hesitate to seek help from maintainers and the community. They are as willing to help me as I would in OSS projects I help maintain.
- You can hack your way through almost anything.