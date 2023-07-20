---
title: Power based Side Channels with Homogenous States
description: A retrospective on the paper "Power-Based Side-Channel Attacks on Program Control Flow with Machine Learning Models"
slug: homogenous-states
image: cover.jpg
date: 2023-07-19 00:00:00+0000
categories:
    - Research Retrospectives
tags:
    - SCA
    - AI/ML
    - Security

links:
  - title: MDPI - View the Paper
    description: The Open Access publication in the Journal of Cybersecurity and Privacy, Power-Based Side-Channel Attacks on Program Control Flow with Machine Learning Models
    website: https://www.mdpi.com/2624-800X/3/3/18
    image: https://www.jobbkk.com/upload/employer/02/B32/02EB32/images/191282.png
---

## Motivation

So this paper was my first submission to a journal, and that experience was a whole interesting one in its own regard, but that's a matter for another day. This paper primarily concerns itself with demonstrating the following: program control flow on low power devices is recoverable via simple power analysis. There had been suggestions in the literature that this was the case, but to the knowledge and research of me and the team I worked with, there had been no verification of this assumption in the physical world.

We began with a separate paper (Transition Recovery Attack on Embedded State Machines Using Power Analysis) which provided the most foundational proof of concept we could come up with. Using two distinct operations, and a state machine with only two states, could we apply simple analysis with machine learning to classify the state transitions that occured. The answer was a clear yes, and after applying additional models to the same dataset after submitting that paper, we were convinced that there were far less strinct bounds on assumptions than might be assumed.

This naturally fed into the question: must different work be performed in the states of a state machine to identify their difference, or is the power utilized during the transition code itself (in other words the control flow code) sufficient to recover the transitions with simple power analysis. More advanced power analysis techniques were of course possible, but for reasons I'll discuss in a bit, we wanted to avoid these more computationally difficult tasks.

## SPA vs *PA

There are a lot of different methods of power analysis discussed in the field of side-channel attacks. Differential and simple are probably the most well known, but others exist (e.g. dynamic, complex, etc.). Additionally, applying deep learning to the task is something quickly picking up in the literature, and it has shown to be quite useful.

However, the computational complexity of applying deep learning models to ML data is something we see as exponentially higher than applying simple power analysis. Since it's the current state-of-the-art, many projects use deep learning to great affect without explicitly discussing the potential for using simple power analysis techniques instead. We had the ability to generate massive amounts of data, so deep learning would have been a strong option. We even had access to large amounts of computational power, so resources were not the concern. Time and the capabilities of the system were our largest issue.

More complex control flow may one day necessitate using deep learning to be able to recover it, I'll certainly admit to that, but, at the moment, simple power analysis is perfectly capable of performing the attacks described in both of these works. I think there's value in using a simpler model simply by the nature of it being more simplistic. It's easier for other researchers to build upon and utilize clear findings than it is to build upon and extend a complex deep learning system. As AI and ML become larger and larger, explainability seems to be taking a back seat in favor of powerful predictive power. In many applications, this is perfectly fine, but in low-power or embedded security, I would rather have the simpler and more explainable model when they are capable of strong results.

## Attack Vector Realization

At this point, I'm beginning to wonder just how applicable this attack would be as an attack vector in the "real" world. For instance, being able to identify and recover transition information for the state machine running your smart lock or a streetlight could be sensitive information, but is it actionable information? Possibly.

This information in and of itself probably isn't going to be hugely detrimental, but as the trigger system for another attack, it does seem to be worth protecting. When paired with a properly instrumented injection attack, it could potentially allow an attacker to force the state machine through a specific execution pipeline.

Now this attack would be far more dangerous. If your smart lock can be diverted to the "open" state when you enter the wrong code or a streetlight can be forced into the "red" state when it should be green, an attacker could cause real damage. This doesn't even address the topic of internet connected medical devices which we use as a potential motivation in the first paper on this topic.

The problem is, we haven't seen this type of an attack yet. While we're able to fully recover state transition information, it's all in the aftermath of execution. An online algorithm, which can take live signals and flag _when_ a transition is occuring would be hugely valuable from a security research perspective.

## Future Directions

This online algorithm is what I see as the best future direction. Some of my co-authors have other directions, which are discussed more in the paper, and which have been raised as valuable future research directions by others in the field. However, in my view, they are foundationally the same question just being explored to find the bounds of this type of attack. This work is undoubtedly important, but I find myself first wishing for a longitudinal demonstration of this type of attack before expanding to determine the full potential attack surface. If no possible attacks are able to use this information, what should designers do to protect the information? Is the answer nothing? That would be a bad one in my opinion.

This difference in future direction seems very natural though. We initially collaborate to prove something possible and then we all have slightly different ideas to explore within that space. I have no doubt that I'll benefit from their research and I hope they will benefit from mine.

## Final Thoughts

Overall, I believe this paper to be my best to date (not hard when it's the fourth to date as well). Personally I loved the sections on how we generated the data, because there were some considerations about efficiently instrumenting the ChipWhisperer Nano for large scale data collection. The python interface leaves some things to be desired, and we built around it as best as possible.

Please support the official publication. It's open access! See the link below for the full article.

> Photo by [Jorge Salvador](<https://unsplash.com/@jsshotz?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText>) on [Unsplash](<https://unsplash.com/photos/c6hEUfgiwnw?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText>)
  