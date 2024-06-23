Title: Lurker to maintainer pipeline
Date: 2023-07-29
Category: posts
Tags: oss, opentelemetry, contributing

<img src="{static}/images/opentelemetry-horizontal-color.png" style="float: right; max-width: 40%; max-height: 300px; height: auto; padding: 0 1em 1em" />

Microservices[^1] were a zero-interest rate phenomenon ([ZIRP](https://en.wikipedia.org/wiki/Zero_interest-rate_policy)). This was the time when it was controversial not to do microservices. Every team had a few engineers who would passionately advocate for moving to this new cool architectural pattern in the town that solves all of your problems, technical or otherwise. I worked at a small startup that had followed the trend and created a un(necessarily?) complicated system[^2]. The development and debugging were a nightmare. The search for missing astra in my quiver as I decided to ride into the battle of the [k8shetra](https://en.wikipedia.org/wiki/Kurukshetra_War) led me to OpenTelemetry.

The following is the rough timeline of my involvement with the project:

## It's always the outdated docs

The unfortunate truth about the project under active development is that the examples are often broken. With every release, many new errors are thrown at you mercilessly. The silent errors make you question your sanity and life choices. OpenTelemetry was no different. Given the pain I had to go through, I decided to send a pull request fixing the example, like a humble squirrel wet its body and [roll over the sand](https://youtu.be/gKcOjnDJfzk?t=4171) over the bridge, while the monkeys pushed the big rocks to build the setu.

## When the universe conspires

To escape the monotony and misery of the world around me caused by the sudden lockdown from the pandemic, I tried to pick up new hobbies to kill time and keep myself busy in isolation. After many failed attempts, I settled with cycling and a blue screen. Over the next few months, my project involvement proliferated. It was like unpaid full-time work outside $job; I took anything that came my way because I was really "vibing" with the project.

The lurker to credible contributor transition is probably the most common path to becoming a maintainer. It starts with being a user. Through intrinsic motivation, you start getting involved in the community. Start by answering questions on Slack, StackOverflow, or GitHub issues. Answering some questions or triaging issues requires understanding the framework/library/tool beyond the surface level. This is where you start digging into the internals. The natural progression is to start fixing bugs and sending patches to missing pieces you find.

## Earn trust

People notice and appreciate your efforts when you consistently show up and do the work. This is universally true. I was no exception. The experts or the "expertise" in something is often a consequence of the time and effort you put in - at least, that's what I believe. 

My commitment to the project was evident from my contributions, code or otherwise. I was offered to become an "approver". During this time, I learned a lot about how to be an effective reviewer (You should read this [https://google.github.io/eng-practices/review/](https://google.github.io/eng-practices/review/) in case you haven't already). And for more than a year, I worked on making substantial code contributions, reviewing and collaborating with diverse people, which helped build trust and confidence in my work. When one of the maintainers decided to step down, I was offered to take over the role.

## #soreadytohelp

As an outsider, I had expected that there would be plenty of contributors. Even if 1% of the users contribute, it would be many people. And that is true, but number of contributors who stick around is tiny.

There are three kinds of contributors: 

1. Those who express interest in contributing but never do or fail to follow through 
2. Those who contribute once and never come back, and 
3. Those who stick around and become regular contributors

The first two categories are the majority. The third category is the one that is the most valuable. They are the ones who are the most likely to become maintainers. You would only know who is who once they stick around for a while. So it is essential to treat everyone as the third category.

Out of around a dozen people over the months, one person stuck around and became a maintainer.

<img src="{static}/images/shalev_maintainer.png" style="float: middle; max-width: 80%; max-height: 1200px; height: auto; padding: 0 1em 1em" />

## Why step down?

I stepped down from the role of maintainer after a year. There are many reasons why I decided to step down.

- The guilt of not being able to justify the responsibility was the biggest one. 
- One can devote only so much time and energy to a project. The return on investment of my time and energy was diminishing. I was not learning anything new or enjoying the work as much as I used to.
- The priorities in my personal and professional life changed.


I had a lot of fun and learned a lot of things. I am grateful to the project and the people involved. It boosted my confidence and self-esteem and helped me grow multi-fold.

[^1]: While the microservices are still a thing, the hype has died down, and people are more pragmatic about it [https://twitter.com/GergelyOrosz/status/1247132806041546754](https://twitter.com/GergelyOrosz/status/1247132806041546754).

[^2]: This was later rewritten as a monolith in another language.
