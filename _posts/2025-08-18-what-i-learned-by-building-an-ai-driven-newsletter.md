---
layout: post
title: "What I Learned by Building an AI-Driven Newsletter"
date: 2025-08-18
description: "Lessons from building an AI-driven newsletter — tools, workflows, and what I learned about automating content generation with large language models."
tags: [deep-learning]
categories: blog
---

*Originally published on [Medium](https://medium.com/@jacksaunders909).*

---

One of the hardest parts of being a researcher in the current world of AI is keeping up with the insane volume of new work that seems to…

---

### What I Learned by Building an AI-Driven Newsletter

![](https://cdn-images-1.medium.com/max/800/1*j7Syf6tXCzlgZS_6MEYiqw.png)

One of the hardest parts of being a researcher in the current world of AI is keeping up with the insane volume of new work that seems to come out every day. I wondered: could I use AI to automatically summarise new papers as they come out? This led to the development of a pretty basic project to build a daily [newsletter](https://realsyncai.com/newsletter). I had a great time building this and thought I would share some of my learning from the project.

![](https://cdn-images-1.medium.com/max/800/1*f9S6X4SQjia9bgdIcV50mA.png)

---

### Keep a human in the loop for now

AI makes mistakes. For most of this article, I discuss some of the ways this happens and how to mitigate it. Yet, no matter what you try, AI is non-deterministic and prone to hallucinations. It might work perfectly ten times and then do something completely unpredictable on the 11th. For this reason, I always make sure to manually approve each newsletter before it goes out. This has helped me catch several pretty substantial errors, which are usually quite easy to correct.

---

### Not everything needs to be agentic

![](https://cdn-images-1.medium.com/max/800/0*wCqhpiU1f1fUE7Jn)

Agents are **the** buzzword right now, and while the jury is still out on exactly what they are, most definitions agree that these are AI models with the ability to select which tools/other agents to call and when, as well as to terminate themselves.

This sounds amazing, and it was what I decided to try for my first attempt. However, I found this far too prone to errors. For example, say I have a function to search the news, and another to search for papers, even though the agent may make the appropriate tool call 95% of the time, this still isn’t good enough, as it will break the flow of the script. You could mitigate this with well-designed error handling, but honestly, it could be more effort than it's worth.

> You should ask yourself, is this really a use case that needs an agent, or would a workflow be better.

For my newsletter, the objectives are fixed: collect data, filter it, summarise, and send. That’s a workflow, not an agent. An agent adds complexity without real benefit. Once I switched up the mindset, I was able to develop the newsletter much more effectively.

---

### You can usually find APIs to do most tasks you want

It’s tempting to rabbit hole on a project like this and to try and build everything yourself from scratch, especially when an optimistic ChatGPT will tell you exactly how to do it (even if it's wrong). However, in many cases, you’re just reinventing the wheel. When someone has already solved the task for you, you might as well use their solution! In many cases, there will even be free APIs for your task (provided you’re working at a small scale). For example, I used:

- [Brevo](https://www.brevo.com/?r=t) for the newsletter structure. This lets you manually design a template and then offers a Python API to fill in sections programmatically and send emails to a list of addresses. It also manages my contacts list.
- The [arxiv API](https://info.arxiv.org/help/api/index.html) to look at recent releases. This brings in a list of papers which you can filter to find relevant articles.
- [ThenewsAPI](https://www.thenewsapi.com/) to get news articles. This allows you to filter by keywords, for me “Digital Humans” and “Avatars” over a certain time period.

However, not everything has an API. For example, I like to use [Scholar Inbox](https://scholar-inbox.com/) to help me find papers specific to Digital Humans, but it doesn’t have an API. Instead, I found a way to read the content from my emails with the Gmail API, but this might be one area I want to develop my own algorithm for. Nonetheless, the APIs that do exist have helped save me countless hours on this project.

---

### Even the best prompts aren’t foolproof

At this point, prompting is more of an art than a science, and it helps enormously to be very precise about what you want. However, even when you are as precise as possible, the LLM will still make mistakes. To illustrate this point, here is a part of the prompt I use for the LLM summarising the papers:

```
Please return the output in the following format:{"TITLE": {"Contribution 1": "…","Contribution 2": "…","Contribution 3": "…","TLDR": "…"}}Make sure that what you return is a valid json object.DO NOT include any other text in your response.
```

Yet, despite this, it will still, on occasion, spit out some extra text about being an LLM, or miss a comma and not be valid JSON. It is therefore very important to include robust error handling for your model's outputs.

---

### The power of a solo founder/developer is better than ever…

This was a fairly small side project, but I was impressed by how easy it was to get off the ground! There are so many tools now that mean you no longer need teams for everything. It’s possible to outsource areas of your app's development to tools to significantly boost your dev speed. For example, I used the following tools to save a lot of time!

- [**Lovable for the frontend**](https://lovable.dev/?via=jack-saunders): I have absolutely zero design skills, so this was a godsend. In just a few minutes, I was able to produce a halfway decent website.
- [**Windsurf for coding tasks**](https://windsurf.com/)**:**While I am a semi-competent developer, I still appreciate the time-saving abilities of an AI-enabled IDE. Lots of the simple tasks were taken care of, freeing me up to focus on the big picture.
- [**ChatGPT for Ideation:**](https://chatgpt.com/)****I found using a chat-based LLM really helpful as a soundboard to bounce ideas off and to help me find the best APIs for a given task.

The list of tools is endlessly growing, and these are by no means the only options for each. Spend some time at the start of your project just messing around with these tools, and you might be surprised by what they can offer.

---

### …but there’s still a way to go.

If you read some of the hype around AI, you might be tempted to think that a single prompt in the right tool can build you a functional and profitable startup. However, this is far from the truth. Current AI tools excel at doing small and well-defined tasks, but break down quickly with project complexity. I did, in fact, try to purely vibe code this newsletter, and the resulting app was utterly useless. At least for now, AI is an assistant or maybe a junior team member, but there’s a way to go before it can replace a full team.

---

Overall, this has been a very interesting project, which has taught me a lot about the state of AI agents and modern LLMs. It’s also been a great help to me for deciding which papers are worth the time investment of a full read.

For me, this project was proof that AI is an incredible accelerator but not a replacement for structured thinking or human oversight. If you’re building your own AI-driven tools, I hope these lessons save you some time.

If you’re interested, you can subscribe [here,](https://realsyncai.com/newsletter) and I plan to keep developing!

By [Dr Jack Saunders](https://medium.com/@jacksaunders909) on [August 18, 2025](https://medium.com/p/f78f927e1f6e).

[Canonical link](https://medium.com/@jacksaunders909/what-i-learned-by-building-an-ai-driven-newsletter-f78f927e1f6e)

Exported from [Medium](https://medium.com) on March 18, 2026.