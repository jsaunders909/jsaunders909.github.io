---
layout: post
title: "Avatars and the EU AI Act"
date: 2024-10-29
description: "How does the EU AI Act affect AI avatars and digital humans? A breakdown of the regulations, risks, and obligations for avatar technology companies."
tags: [digital-humans, ai-policy]
categories: blog
---

*Originally published on [Medium](https://medium.com/@jacksaunders909).*

---

The EU has announced a series of regulations for AI companies; how will this affect lip-sync/avatar/deepfake providers?

---

### Avatars and the EU AI Act

### The EU has announced a series of regulations for AI companies; how will this affect lip-sync/avatar/deepfake providers?

![This image is AI generated using DreamStudio.](https://cdn-images-1.medium.com/max/800/1*-x4kINigeih_f3tHvZNurA.png)

The EU AI Act came into force on August 1st, 2024. It will become enforceable over the next two years. There's a lot in there, from introducing codes of conduct to outright banning specific tech. As someone deep into Avatar research and development, I wanted to understand what it means for those working in the space. This article is me doing that and trying to share what I’ve found. Let me start with a huge disclaimer:

> I am not a lawyer, this is not in any way legal advice. Everything in this article is my own take on what I’m reading and is meant to raise awareness and start conversations. Please seek legal advice if you think anything here might affect you.

With that out of the way, let's take a look.

### Who Does it Apply To?

The act lays out who is affected right at the start. It states that providers, users, distributors, importers, and manufacturers are all affected. What are all of these categories? It's a dense read with a lot of legal jargon, but in essence, any person (or business) who operates in the EU is affected, as is any person or business based **outside the EU** that puts an AI model on the EU market. This is true of commercial products but appears to be true of free systems as well.

> Basically, if you’re an entirely non-EU company or individual, and you restrict access to your AI model so that no one in the EU can use it, you might be ok. Otherwise, this **will** apply to you.

In more detail:

- A **provider** is: “A natural or legal person, public authority, agency or other body that develops an AI system […] and places it on the market or service […], whether for payment or free of charge”. Effectively, these are the developers. ***It’s worth noting this includes those doing this for free. So researchers would be included.***
- A **deployer** is: “a natural or legal person, public authority, agency or other body using an AI system under its authority.” Effectively, these are the users and creators making videos with these tools.

Other categories apply, but we’ll focus on these two as they will make up most of those reading this article.

---

### Risk Based Classifications

![](https://cdn-images-1.medium.com/max/800/1*ctqBdJqmdHR2Up-kxt40-w.png)

The act adopts a risk-based system for classifying AI models. Some models, particularly those designed to deceive, are outright prohibited. Models deemed high-risk, such as those in specific areas like education, employment, or law, must follow strict rules regarding data, documentation, and more. Lower-risk models are only subject to transparency requirements.

From my first read of this, I had feared that lip-sync-like models could fall under the prohibited category, as they do in some way deceive by creating realistic-looking synthetic content. However, fortunately, we do not need to try to work out which category avatar models fall under, as we have the following definition in Article 3(60).

> ‘deep fake’ means AI-generated or manipulated image, audio or video content that resembles existing persons, objects, places, entities or events and would falsely appear to a person to be authentic or truthful

‘Deepfakes’ are explicitly mentioned as low-risk and subject to transparency requirements. Let’s examine them.

### Deepfake Transparency Requirements

The following two paragraphs of Article 50 appear to be the most relevant for our field.

### 50(2) — Providers

> Providers of AI systems, including general-purpose AI systems, generating synthetic audio, image, video or text content, shall ensure that the outputs of the AI system are marked in a machine-readable format and detectable as artificially generated or manipulated. Providers shall ensure their technical solutions are effective, interoperable, robust and reliable as far as this is technically feasible, taking into account the specificities and limitations of various types of content, the costs of implementation and the generally acknowledged state of the art, as may be reflected in relevant technical standards.

This applies to those who develop these systems. If you are a company building your own model, this is you. If you are using an API as part of a system and placing it on the market, it is less clear, but I still think this applies. This also appears to include developers of open-source models.

Most noticeably, your system needs to be marked in a machine-readable format. I see three options for this:

- **Metadata:**By adding a label to any generated files' metadata, your model is labelled in a machine-readable format.
- **Watermarking:**Adding a visible or invisible watermark should also work.
- **Deepfake Detectors:** You might be tempted to argue that a Deepfake detector can detect your model, so it is machine-readable. I disagree with this argument as not everyone will have access to detectors, and they will not be perfect. I would not recommend relying on this.

Obviously, the first is the most straightforward and cheapest, but someone wanting to misuse your model could very easily remove it. The act says the method must be “robust.” To me, it is not clear what precisely this means. I discuss this further in the article.

There is a promise of “Relevant Technical Standards”, but I am unaware of these.

---

### 50(4) — Deployers

> Deployers of an AI system that generates or manipulates image, audio or video content constituting a deep fake, shall disclose that the content has been artificially generated or manipulated. This obligation shall not apply where the use is authorised by law to detect, prevent, investigate or prosecute criminal offence. Where the content forms part of an evidently artistic, creative, satirical, fictional or analogous work or programme, the transparency obligations set out in this paragraph are limited to disclosure of the existence of such generated or manipulated content in an appropriate manner that does not hamper the display or enjoyment of the work.

This part covers deployers — these are the models' users. So if you are using an Avatar, ranging from open source like [wav2lip](https://medium.com/@jacksaunders909/wav2lip-generalized-lip-sync-models-e0effc4e8ed3) to some of [the commercial companies](https://medium.com/@jacksaunders909/the-definitive-guide-to-lip-sync-companies-8a2fc4f318e2), you’ll be affected.

For deployers, you must disclose that the Avatar you are using has been generated. For example, suppose you run a business that uses Synthesia to create a training video for your employees. In that case, you must include that the avatar is AI-generated somewhere in that video. I believe the intent is that anyone who sees the video can tell it's generated, unlike providers, who must make it machine-readable.

---

### My Questions

Reading through the act, it seems there are many unanswered questions.

### How Robust Does Labelling Have to Be to Adversaries?

As we discussed, anyone who creates what can be referred to as a deepfake must label the content as AI-generated. Article 50(2) states:

> Providers […], shall ensure that the outputs of the AI system are marked in a machine-readable format and detectable as artificially generated or manipulated.

A straightforward way would be to add labels to the top of the generated image or video or add a metadata tag. The problem is an adversary could easily remove this.

![An oversimplified adversary problem. Is the person who generated the left image responsible if someone crops out their mark? By the way, this image is itself AI-generated using Stable Diffusion.](https://cdn-images-1.medium.com/max/800/1*lNcNaX7fVVHa9iuIF8iEAA.png)

The act states:

> Providers shall ensure their technical solutions are effective, interoperable, robust and reliable as far as this is technically feasible.

This leaves a lot of room for debate. In particular, I think there are different interpretations of what robust means. The above example would be robust to accidental attacks such as compression but very easy to remove if you tried.

But even an advanced AI watermark could easily be broken by someone with sound technical knowledge. To what degree does the labelling need to be robust? The EU Commission may release more guidance on this in an upcoming meeting, but this will remain a complex question.

### Who Exactly is Affected Again?

Recall that the Act will bind anyone who makes an AI model available in the EU. This is an extremely wide scope. In particular, I’m thinking about people who create open-source models on GitHub. If you make a public repo, it’s available in the EU. So, for example, would a Chinese researcher making their code public for acceptance to an American conference be fined? This seems both unfair and hard to enforce.

### How Uncompetitive Will This Make EU Companies?

One of the biggest concerns I hear repeated about the Act is that the EU is shooting itself in the foot. They say that this will make EU AI companies less competitive. However, speaking from a purely Avatars/Lip-Sync standpoint, I’m not so sure this is guaranteed. It will depend largely on how the standards are implemented, but if adding a metadata label and a visible watermark is sufficient, I don’t foresee any disadvantages. The question is whether the labels must be robust to adversarial attacks. If so, this could be expensive, increasing the operating costs of Avatar companies in an already very tight and competitive marketplace.

### What Now

So what can you do about it? I once again want to reiterate that nothing I say is legal advice, but this is what I would do:

- **For Everyone:** I would sign up for the [European Commission’s AI Pact.](https://digital-strategy.ec.europa.eu/en/policies/ai-pact) This will hold webinars and invite discussion on exactly what standards will be implemented. Participate in the discussion and shape the regulation.
- **For Companies:** For now, I would add metadata information to every video saying, “This video was generated by X”, where X is my company. I would also ensure that there is a clear text saying “AI GENERATED” anywhere end users see my videos. Personally, I would include this somewhere in the actual video frames **AND**on any webpage/app that displays videos. I would also keep an eye on watermarking methods.
- **For Hobbists:**If you use these systems for personal projects, you should make sure any videos you share are clearly labelled as AI-generated. For example, in a social media post, include the phrase “This is AI-generated.” in bold.
- **For Researchers:** On any project pages, I would have AI GENERATED somewhere in the caption of any generated videos. Also, in presentation videos/demo videos, I would have clear text saying the same. For GitHub repos, I would add a section to my inference code that includes a textbox to say AI GENERATED in the final video frames.

For myself, I intend to do some more research into watermarking methods and engage in discussions to help me better understand the Act. Watch this space!

By [Dr Jack Saunders](https://medium.com/@jacksaunders909) on [October 29, 2024](https://medium.com/p/b3c390eb7b8a).

[Canonical link](https://medium.com/@jacksaunders909/avatars-and-the-eu-ai-act-b3c390eb7b8a)

Exported from [Medium](https://medium.com) on March 18, 2026.