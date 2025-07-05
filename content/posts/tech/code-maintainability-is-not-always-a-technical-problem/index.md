---
title: "Code Maintainability Is Not Always a Technical Problem"
date: 2025-07-05T18:19:58+02:00
description: "When it comes to debating about code maintainability, I often hear about software architecture, design patterns, testing, refactoring, etc. But I find it only partially true. No matter how good your software design is, no matter how good your tests are, if you work in a big team on a big project, it won't save you from collapse. Here is my take on code maintainability and things that I don't hear other people talk about."
keywords: ["maintainability", "software", "code", "CI"]
draft: false
params:
  tocEnabled: true
  coverCredit: "Stockholm. September 2018"
---

# Context

First, who am I to even talk about it? What is my background?

I've been building software for the last 15 years. I've worked at 8 companies ranging from small startups to one of the biggest tech companies in the world.

At the moment I primarily work on [Beats](https://github.com/elastic/beats "Elastic Beats"). A few facts about this repository (July 2025):

* ~15 teams work on it
* 918 unique contributors (including external ones)
* 4,183 Go files
* 653,168 lines of code
* 45,205 pull requests

So, it's a relatively big project with quite a few people working on it. Let's take my path from onboarding to the present day as an example for this article.

# On-boarding

I joined in November 2021 and opened my first PR about a month later. This was quite a long on-boarding time, and there's a reason for it.

The nature of the product makes it quite complex — data ingestion from various sources to various outputs with data processing in between. But also, because so many people work on this project, they approach problems very differently and write using very different code styles. There was a time when I thought I finally understood the pattern but went to another file and...

> Nope. Here it's completely different, so what pattern should I follow in my PR?

Then I open a PR with what I think was the most common pattern I followed just to learn nobody is supposed to write like so anymore. The reviewing team does not accept it.

What could help to solve this?

A few months later, in March, I finally had enough and started my initiative of adding the [golangci-lint](https://github.com/golangci/golangci-lint) to the repository.

# Introducing Go Linter

To some people it might sound simple, but adding a linter includes:

* I need to align a dozen teams on one set of linter rules
* Figure out automation on PRs
* We need to address already existing issues in the repository

Adding the linter was supposed to address a subset of the problems, not to solve all of them. A few examples the linter would help you with:

* There is a security issue in your code
* You don't check a returned error
* You use a deprecated function
* You try to use a banned dependency
* Dead/unreachable code
* Repeated values that can be replaced by a constant
* Typos in the comments
* `context.Context` is not used but should be
* ...

So, at least it would reduce the load on the code reviewers and would give contributors more confidence.

## Alignment

Naturally, the alignment on the same set of rules took some time. Eventually we got there, and the PR was merged 5 days later. Better than I anticipated.

Not a technical problem at all, as you can see.

## Automation

GitHub has a built-in action for this linter, and setting it up for pull requests was not challenging in any way. The tricky part was the repository size. When we tried to run the linter against the entire repository, it just crashed. It was literally too much for this poor program.

The only feasible solution I could find was to run it against **some** files at the time. A commit changes? Pull request changes? I added a local command for everyone that could run the linter on the diff against the `main` branch. This helped to identify issues in the local changes even before opening a PR.

For already opened PRs, we just limited the scope of checks to all files edited by the current PR. This also offered us a solution to the next problem.

## Fixing Existing Issues

It was absolutely unrealistic to solve all the linter issues in the entire repository, so proposing that was not even on the table.

Instead we made an agreement on our team:

* You touched the files in any way: you solve all the linter issues in them. Unless some emergency/blocking changes.
* This does not apply to external contributors; it's unfair to put this burden on them.

Some people would say it's too much to ask, but in the end it all depends on your perspective.

If you think about it as:

> I have a chance to fix a few issues on the way. This will make our code more maintainable and resilient.

and **not**:

> Oh, that contributor made so many silly mistakes! Grr, why do I have to do this?

then it should work out just fine.

Yes, it's annoying that you have to fix issues after someone else every time you make a small change, but this pays off in the long run. It exposed countless amounts of missed error checks and other kinds of bugs and made our code base better over time. And it continues to do so even now.

Again, a non-technical problem, as you can see.

## Learnings

It does not matter how obvious a certain improvement is to you. You have to be a very good salesperson in order to convince a group of people that something is worth the big change. People are normally resistant to change. Change coming from someone else invades their world and is treated as such. Keep it in mind.

There were a few learnings on the way:
* It's better to collect a few situations (links, screenshots, etc.) where having a linter would have helped. It's hard to argue against such evidence.
* Humans are more likely to follow demands from a machine than from another human. An automation is seen as a neutral party. We all agreed on the set of rules, so people feel obliged to follow them. Changing rules needs a realignment, and not everybody wants to deal with that unless it's worth the effort when something is really wrong.
* Set realistic expectations: don't expect to halt everything and dedicate a few months to fixing linter issues. This is not happening. Instead, try to reduce the scope and come up with a way to iterate on the issues.

These are the main learnings from this story, and all of them are about collaborating with other people.

# Three Years Later

Three years after the linter work, I was asked to drive another effort in [Elastic Agent](https://github.com/elastic/elastic-agent "Elastic Agent repository").

This time it was a flaky test problem. At some point our **CI reliability numbers dropped to 10%**. Occasional fixes here and there were not cutting it anymore, and our team needed someone to work on this full time. I volunteered. Little did I know what I was getting myself into.

## From Total Chaos to Order

Dozens of tests were failing; in different CI runs, different tests failed. Services we depended on were failing during our test runs. Even using the Google Cloud Provider API was failing at times.

This looked very overwhelming, so I started with setting the right processes in place and trying to order the chaos:

* I created an issue template for reporting a flaky test, wrote a guideline on how to report it, and shared it with the team.
* I created a project dashboard with automation around such issues.
* I set up weekly reporting (semi-automated in a spreadsheet) about the origin of failures, categorized. Pie charts, statistics, and so on. This became a section of our department's weekly update email.

Suddenly, we had a clear overview of what exactly is contributing to these CI failures. What contributes the most and where we need to focus our attention.

The biggest source of flaky tests was the way we get the list of versions of our product. You see, in Elastic Agent we have the `upgrade` command. We support upgrades from a range of previous versions to the latest one. We have to test all combinations of upgrades and downgrades to make sure our customers would not brick their environment in the process. This includes unreleased daily snapshot versions too.

The version set is constantly changing because of new snapshots or public releases, and the decision was made to just access our version API, in each run and request the current set of versions. This is supposed to be an internal-only API and nobody ever thought about its scalability. When I talked to the owning team about this issue, they even told me that the API is deprecated and I should've used something else. Oh, well.

I mitigated this issue by introducing a daily GitHub action that fetches the relevant version set and opens a PR with editing a static file in the repository. All tests now use this static file instead; the version API does not crash. The version set is reviewed, approved, and merged only if it actually works. This is how we significantly reduced the number of flaky tests.

## Lessons and Learning

This work required me to:

* Collaborate on my findings across teams
* Coordinate other engineers who contributed to the solutions
* Take one rabbit hole after another almost weekly

In about 6 months we finally started to achieve high reliability numbers again, so I could switch to working on something else again.

This work was not easy, but it exposed some important company-wide issues and pointed to some common mistakes developers make while writing tests. Only by walking this painful could we path have these important learnings:

* Sometimes a big problem cannot be solved unless you have a structured overview — it's worth spending time on this first. Don't just skip and rerun; document.
* Every remote service fails at some point, even Google Cloud, even when your tests are also running on Google Cloud. Put retries.
* Reduce your runtime dependencies to a minimum or to what you actually want to test together in integration. Prepare/upgrade the testing environment in daily jobs.
* Don't use time as a part of an identifier. Ever. No. Please don't. Use UUID. Strive not to rely on time in tests in general.
* If your tests fail only on certain hardware, it's possible the hardware is defective. Yes, really. Say hi to early ARM Ampere machines for me.
* CI reliability is not a one-time fix. It's an ongoing effort the entire team should be contributing to. Set the right expectations and communicate to the team.

There were more; these are highlights.

Eventually, everything came down to setting the right processes in place, lots of communication, collaboration, and mentoring team members on certain topics. Although at times it was about technical problems, but the human aspect of it definitely exceeded in its effort.

# Not Always Technical

I hope these 2 short stories gave you a fresh perspective on code maintainability. It's not always the software design, it's not always the programming language, and it's not always the presence of tests. There is a huge human aspect contributing to code maintainability. Most of the time it's all about team agreement, knowledge sharing, mentoring, and documenting.

## Agreement

Depending on the size of your team, it can be very challenging to agree on something. I've seen 3 developers with absolutely opposite points of view. Some people just want to be heard, want to contribute to the conversation although they don't bring anything new to the table. Big discussions are very inefficient. However, it's a delicate balance: tell others what to do, and you might miss a very valuable perspective, or you risk working with passive-aggressive colleagues.

In my experience, I find it quite effective to start with a small group, maybe even 1 on 1, trying to sell your solution. Then expand your workgroup, and when it comes to a vote, most of the team members would already be on board. You also collect valuable feedback and iterate on the solution while doing so.

## Knowledge Sharing

In order to make an impact in certain areas, you would need to communicate your knowledge clearly. This is as important as any technical skill. Good storytelling, clear presentation, and confidence are crucial.

Make sure your presentation skills are sufficient. Collect feedback and work on it. It's not about you; it's about how useful you can be to the team.

## Mentoring

The best way to solve a long-running issue on the team is to start planting seeds in mentoring and collect the harvest later.

Do you see certain team members doing something that causes trouble? Volunteer to mentor new hires or even established team members and help them understand the issue by giving clear examples. Let's be honest, not everyone pays attention to presentations on a group call. It's much harder to ignore something when you're in a 1-on-1 mentoring session.

## Documenting

I'm not going to reinvent the wheel here. Just document your findings in the code, in the README, in the PR description, etc. Just have a publicly available place that you can link to in case you encounter a known problem. Everything communicated verbally during a call is lost by default. Make a summary write-up. Decisions made during calls need to become an issue or a comment if you really want anything to get going.

# Final Words

I once had a conversation where someone said:

> Code is maintainable when it's well-tested and the design is right.

I hope I convinced you that it's only partially true. One needs to become a good salesperson, an excellent communicator, a confident presenter, and a good mentor in order to create a culture of maintainability on their team.
