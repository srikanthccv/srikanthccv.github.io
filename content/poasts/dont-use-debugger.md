Title: Say no to debugger
Date: 2023-09-17
Category: poasts
Tags: debugger

Last Friday, I pair-programmed with my co-worker [Dhawal](https://github.com/dhawal1248) to debug an issue. While debugging, it came up that I don't use a debugger. I didn't have a good answer When they asked me why. I have been thinking about it since then, and I think I have an answer now. Broadly speaking, there are two reasons why I don't use a _real debugger_[^1].

## Never learned to use one

I wrote my first computer program in C during my first year of college. The tools I knew then were [GCC](https://gcc.gnu.org/), [gedit](https://help.gnome.org/users/gedit/stable/) and a command line. I would often run into segmentation fault crashes. The only way I knew to fix the issue was to add `printf` statements or throw away the code and start over. Looking back at it, It seems like a very inefficient way to debug, but I am glad I never got used to using a debugger.

## Print + Read Aloud is all you need

The habit of not using a _real debugger_ continued into my professional career. Whenever I needed to debug something, I would simply stare at the code and read it aloud in my head. (Not so) surprisingly, carefully reading the code aloud and thinking about it would often lead me to the issue. Or, to put it another way, using your brain and mouth to "step through" the code is very effective.

The "Stare into code" approach doesn't always work and requires that you already know the part of the code that is causing the issue. So, what do you do when you don't know where the issue is? I would come up with a hypothesis about the issue. I would then add `printf` statements to the code to verify the hypothesis. Repeat the process until I found the issue. I found this method to be very effective. I didn't know this process has a name until I read [https://eikmeier.sites.grinnell.edu/csc-151-s221/readings/hypothesis-driven-debugging.html](https://eikmeier.sites.grinnell.edu/csc-151-s221/readings/hypothesis-driven-debugging.html). I highly recommend reading it.

My approach to debugging has evolved over time. The first thing I do now is write a repeatable test case that reproduces the issue[^2]. Any time that goes into writing this test case is well spent because you will have to run the code over and over again to verify your hypothesis. In the end, you have a test in your suite to ensure this bug doesn't crop up again. In my experience and from what I have seen, those who don't write this repeatable test are the ones who reach for a debugger first.

Once the test case is ready, I would start at a high level and work my way down to the issue. This is where I think the _print debugging_ differs most. With a _real debugger_, you would set a breakpoint and step through the code. The problem with this is that it is easy to get lost in the code and details that are not relevant to the issue. Even if you step over function calls, it is seldom a wrong level of detail. As I narrow down the issue, the state of variables becomes important. I would then add `printf` statements to reject or confirm my hypothesis. Repeat this process until the issue is found. 

## Final thoughts

The tools we use shape the way we think. I think the _print debugging_ approach has helped me become a better programmer. Though this sounds obvious - It forces me to read the code and understand it. The thought + read aloud combination helps me think about the code in a different way. The ability to read through the code and spot issues has made me a better reviewer. I believe debuggers are very useful tools, and I am not against using them. I just think that they are not the first tool to reach for. I would like to think a debugger should be used only when one runs out of other options. Or I probably suck at using a debugger.

[^1]: Debugger is the last resort. I rarely use it, but when I do, I either debug a tricky issue or work with a language/codebase that I am unfamiliar with to understand the code.
[^2]: My collaboration in OSS project(s) helped me make this a habit - [Tigran](https://github.com/tigrannajaryan) always asks for a reproducible test case when a pull request is submitted.
