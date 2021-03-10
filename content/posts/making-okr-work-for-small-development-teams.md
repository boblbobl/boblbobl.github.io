---
title: "Making OKR Work for Small Development Teams"
date: 2020-09-28T09:48:26+01:00
draft: false
categories:
- 2020
- Work
tags: 
- acceptance criteria
- dashboard
- key results
- objectives
- okr
- team
- way of working
---

A current trend in our industry is to formulate your goals as Objectives and Key Results (or OKR). However, there are a few challenges when implementing OKR, especially on a team level.

This post is an overview of how we use (or misuse) OKR on a small development team.

To give you a little context, our team consists of 3 developers and a product owner (that is I). We have a daily stand-up every day at noon and do refinement session once a week. Everyone (including myself) are attending all team meetings (including retrospectives). We use a Kanban board to support the prioritization of work and tracking progress. We do not use sprints or commit to work that we need to deliver within a specific deadline; instead, we communicate expected delivery dates based on our lead time.

## So why are OKR Cool?

OKR is a great tool to formulate and implement a strategy because it clearly defines a goal for which the team needs to achieve and measures progress towards achieving this goal through well-defined work descriptions using the format of (usually up to 5) key results.

OKR is a well-established framework and has roots that can be traced back to 1954, and is used today by companies like Google, LinkedIn, and Twitter.

## And here is why OKR Suck!

Especially when using it on a team level, it can become a bit too cumbersome to formulate deliveries as goal settings; remembering the real purpose of OKR is a tool to implement strategies. Using the OKR framework to discuss and decide what to do next and track it, on a team level, may feel like you are trying to fit a square peg into a round hole; this is notably the case when defining key results. It is not always possible to find a measurable unit (bugs, uptime, packages, etc.), and setting a goal will often be bound in guesswork.

## How to make it work

We have chosen to use OKR to set quarterly objectives for the team. The objective represents predominantly large chunks of work and something that has a great interest to our stakeholders. For that reason, we use the objectives as our contract with the business (this is what we are building for you guys). When we meet a complete state of any objective, that will trigger a demo session to show what is delivered.

Instead of spending hours formulating and extracting the perfect SMART key result (KR) as measurable indicators of whether the objective is a success, we instead use a much more familiar concept for the team, the Acceptance Criteria (AC), as our KRs.

AC, when used to define the success of an objective, is a verification list, where we can ask if the particular AC is passed or failed. We can use this to indicate when we delivered an objective, or in some cases, to communicate what is missing.

## That is all great, but how do you then communicate progress?

We already have a process of moving stories or, in our case, [GitLab](https://about.gitlab.com/) issues, from start to finish on our Kanban board, so why not associate those with our objective and then use the percentage done to indicate progress?

So this is how this looks:

![A screenshot of team dashboard](/images/posts/teamboard.jpg "A screenshot of our dashboard displayed on a big-screen TV in our team area or through an accessible link")

The screenshot above shows our team board, where we display a snapshot of what we are working on right now, including our quarterly objectives. If we zoom in on the objectives in the left most column, you can see that “key results” are presented as a checklist of things that need to be true, and in the bottom, there is a progress indicator.

![A screenshot of team dashboard zoomed-in](/images/posts/objective-acceptancecriteria.jpg "A screenshot of a zoomed-in view of a single team objective on our dashboard")

Since we use GitLab on the team, this has been developed and is maintained using data inside GitLab. Milestones in GitLab represent objectives, where we use a task list to describe the ACs. We associate all relevant issues to an objective by picking the appropriate milestone in GitLab. The dashboard itself is just a static (Bootstrap) website that fetches data through the GitLab API.

All data shown here are purely mock data or blurred out. The team’s avatars have been replaced with Adorable Avatars.

## Pitfalls

Because we learn during the development of an objective, we will add or remove stories, making the progress indicator fall and raise respectively during the lifetime of an objective. Stakeholders cannot use the progress indicator as an exact percentage of how far we have come or has left.

## Wrap Up

I hope this gives some inspiration on how you might use OKR or a variant of it on your small development team to sign-off on work that needs to be delivered and communicate progress.

I would like to hear what you think about our approach, and if you are doing something similar or have disregarded – or adopted OKR fully. Please leave a comment below.
