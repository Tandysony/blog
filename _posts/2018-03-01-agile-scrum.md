---
layout: post
title: Scrum - Agile Software Development
thumbnail: "assets/img/thumbnails/Scrum-Framework.png"
tags: [Agile, Scrum]
---

Agile software development refers to a group of software development methodologies based on iterative development, where requirements and solutions evolve through collaboration between self-organizing cross-functional teams. Agile methods or Agile processes generally promote a disciplined project management process that encourages frequent inspection and adaptation, a leadership philosophy that encourages teamwork, self-organization and accountability, a set of engineering best practices intended to allow for rapid delivery of high-quality software, and a business approach that aligns development with customer needs and company goals.

When talking about agile, **scrum** is the concept must be mentioned.

## Scrum

`Scrum` is a subset of Agile, an **iterative** software development framework. It is a lightweight process framework for agile development, and the most widely-used one.

A “process framework” is a particular set of practices that must be followed in order for a process to be consistent with the framework. (For example, the Scrum process framework requires the use of development cycles called `Sprints`.)

“Lightweight” means that the overhead of the process is kept as small as possible, to maximize the amount of productive time available for getting useful work done.

## User Story

A `user story` is a tool used in Agile software development to capture a description of a software feature from an end-user perspective. `user stories` are the smallest units of work. It can be represented in this pattern: `As a <type of user>, I want <some goal> so that <some reason>.`. For example, `As a customer, I want to be able to create an account so that I can see the purchases I made in the last year.`

User stories are sketched out by the product owner, and then the entire product team collectively determines detailed requirements. These are the granular pieces of work that help define the implementation items for the story and the upcoming `sprint`.

# Story Points

`Story points` rate the relative effort of work in a Fibonacci-like format: 1, 2, 3, 5, 8, 13, 20, 40, 100. It sounds counter-intuitive. I personally use the following table as a reference between effort in time and the story points.

| Effort in Time | Story Points |
| -------------- | ------------ |
| 0.5 hr         | 1            |
| 1 hr           | 2            |
| 2 hr           | 4            |
| 4 hr           | 8            |
| 1 day          | 16           |
| 2 day          | 32           |
| 3 day          | 48           |
| 1 week         | 100          |

The table makes much more sense and is intuitive to human being, but still keeps the essence of agile. And, we do not need a `scrum master` to do the estimation. Please `avoid create a story that requires more than 3 days, otherwise, break it down.`.

## Sprint

In Scrum, teams forecast to complete a set of user stories during a fixed time duration, known as a `sprint`. Generally speaking, sprints are one, two, or four weeks long. Two weeks is good to start with.
Fixed length sprints reinforce estimation skills and predict the future velocity for the team.

## Daily Standup Meeting

After your sprint has started, have your team meet daily, typically in the morning, to review what everyone is working on. The purpose of this is to see if anyone on your team is experiencing any roadblocks towards the completion of sprint tasks.

Questions to ask:

- What did I complete yesterday?
- What will I work on today?
- Am I blocked by anything?

There's an implicit accountability in reporting what work you completed yesterday in front of your peers. No one wants to be the team member who is constantly doing the same thing and not making progress.

## Sprint Retrospective Meeting

After you complete the sprint, have your team do a retrospective. Document your retrospective somewhere.

Questions to ask:

- What did we do well during the sprint?
- What could we have done better?
- What are we going to do better for next time?

## Software to Use

There are many tools to follow the scrum principles. JIRA is one of them. Refer to [Learn scrum with Jira Software](https://www.atlassian.com/agile/tutorials/how-to-do-scrum-with-jira-software) to learn more.

# References

1.  [WHAT IS AGILE? WHAT IS SCRUM?](https://www.cprime.com/resources/what-is-agile-what-is-scrum/)
2.  [Learn scrum with Jira Software](https://www.atlassian.com/agile/tutorials/how-to-do-scrum-with-jira-software)
