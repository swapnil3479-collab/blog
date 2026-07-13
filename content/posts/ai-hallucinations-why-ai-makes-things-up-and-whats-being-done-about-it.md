---
title: "AI Hallucinations: Why AI Makes Things Up and What's Being Done About It"
date: 2026-07-13
slug: "ai-hallucinations-why-ai-makes-things-up-and-whats-being-done-about-it"
description: "In 2023, a New York attorney submitted a legal brief he had written with help from ChatGPT. The brief cited several court cases as precedent. A judge reviewed i"
draft: false
tags:
  - "AI"
  - "Hallucinations"
  - "LLM"
  - "ChatGPT"
cover:
  image: "/blog/images/ai-hallucinations-why-ai-makes-things-up-and-whats-being-done-about-it-cover.svg"
  alt: "AI Hallucinations: Why AI Makes Things Up and What's Being Done About It"
  relative: false
  hidden: false
---

In 2023, a New York attorney submitted a legal brief he had written with help from ChatGPT. The brief cited several court cases as precedent. A judge reviewed it and noticed something odd — none of the cases existed. ChatGPT had invented them, complete with case names, dates, and judicial opinions that were entirely fabricated. The lawyer was sanctioned. The cases were fake. And the AI had delivered them with complete, unqualified confidence.

That's an AI hallucination. And it's not a bug.

---


![Key Statistics](/blog/images/ai-hallucinations-why-ai-makes-things-up-and-whats-being-done-about-it-infographic.svg)


## What a Hallucination Actually Is

The term comes from psychology, where a hallucination means perceiving something that isn't there. In AI, it means generating information that sounds real but isn't — and presenting it as fact without any indication that it might be wrong.

What makes it genuinely dangerous isn't the error itself. It's that there's no signal the error is happening. When ChatGPT invents a court case or Claude fabricates a research study, the output looks identical to a correct response. Same confidence, same polish, same authoritative tone.

Some researchers think "confabulation" is a more accurate word — it's what psychologists call the brain's tendency to fill in memory gaps with plausible-sounding content without realizing it's doing so. That's closer to what AI is actually doing. But "hallucination" has stuck, so that's what we'll use.

---

## Why AI Makes Things Up

To understand hallucinations, you need to understand what large language models actually do — because it's not what most people think.

LLMs don't retrieve facts. They don't search a database. They don't reason through a problem and check their work. What they do, at the most fundamental level, is predict the next word.

When you ask ChatGPT a question, the model generates tokens one at a time. For each token, it asks: given everything before this, what's the most statistically probable next word? It does this billions of times during training, learning patterns from enormous amounts of text — books, articles, websites, code, forums. It gets extraordinarily good at producing text that sounds like what those sources would say.

The critical part: there's no step where it checks if what it's saying is true.

The model can't distinguish between "this is accurate information" and "this is what accurate information sounds like." When you ask about a historical figure, it generates text in the style of how historical figures are described. When you ask for statistics, it generates text in the style of how statistics are presented. If the specific fact doesn't exist in its training data, it fills the gap with whatever fits the pattern — and that fill is a hallucination.

This isn't a flaw that can be patched. It's a consequence of the architecture that makes LLMs powerful in the first place.

---

## The Confidence Problem

The scariest thing about hallucinations isn't how often they happen — it's how they happen.

When an AI hallucinates, it doesn't slow down. It doesn't add a caveat. It doesn't say "I'm not certain about this." It delivers fabricated content with the same confidence as accurate content because, from the model's perspective, there's no difference. It generated text that statistically fits the context. Job done.

This is why the lawyer's story is so instructive. The fake cases weren't obviously wrong. They sounded like real cases. They had the right format, plausible names, reasonable dates. The model had learned what legal citations look like and produced something indistinguishable from the real thing.

Some of the most famous examples follow this same pattern:

- Google's Bard incorrectly claimed the James Webb Space Telescope had taken the first images of an exoplanet outside our solar system — wrong, confidently stated, and damaging enough to wipe billions off Alphabet's stock value in a single day
- Google's AI Overviews suggested adding non-toxic glue to pizza sauce to keep cheese from sliding, and separately stated that eating rocks is a good source of minerals and vitamins
- ChatGPT advised a user to substitute table salt with sodium bromide — a toxic compound — and the user was hospitalized with a condition called bromism
- The FDA's own AI tool hallucinated nonexistent clinical studies while evaluating drug approvals

In each case, the AI wasn't guessing randomly. It was pattern-matching to what a correct answer would look like — and getting it catastrophically wrong.

---

## How Often Does This Actually Happen?

Estimates vary depending on the model and the task. Research from AI company Vectara puts the range at 3% to 27% across popular chatbots, measured on document summarization tasks. Newer models are generally better — recent large language models tend to hallucinate at 1–3% on controlled benchmarks.

But here's the counterintuitive part: the newer "reasoning" models, which are specifically designed to think through problems step by step before answering, can actually hallucinate more in certain categories. OpenAI's o3 model hallucinated 33% of the time on person summaries — up from 16% in its predecessor o1. The o4-mini hit 48%.

The reason is almost perversely logical: reasoning models "loop" through a longer internal thinking process before generating a response. More loops means more chances to make something up. The very mechanism designed to improve accuracy creates more surface area for errors.

In the real world, the stakes are already showing up. A 2024 survey found that 47% of enterprise AI users had made at least one major business decision based on hallucinated content. In just the first quarter of 2025, over 12,000 AI-generated articles were removed from online platforms for containing fabricated information.

---

## What's Being Done About It

The AI industry knows this is a problem. The question is how much can actually be fixed versus how much is baked into the fundamental design.

**Better training data** is the most direct lever. Models trained on diverse, well-curated, accurate data hallucinate less than those trained on noisy, biased, or thin datasets. The problem is that "better data" is easier said than done at the scale these models require.

**Retrieval-Augmented Generation (RAG)** is one of the most promising practical fixes. Instead of relying solely on what the model learned during training, RAG systems pull in real documents at query time and ask the model to answer based on those documents. This grounds the response in actual sources rather than statistical inference. It doesn't eliminate hallucinations, but it significantly reduces them for knowledge-intensive tasks.

**Human oversight** remains the most reliable backstop. Every major AI company now includes disclaimers — "may display inaccurate info," "can make mistakes" — precisely because the models can't reliably police themselves. For high-stakes decisions in law, medicine, or finance, a human expert reviewing AI outputs before acting on them isn't optional.

**Reasoning models**, despite the irony of occasionally making things worse, represent one direction the field is actively exploring. The theory is that slower, more deliberate reasoning gives the model more opportunities to catch its own errors. The practice, as the o3 numbers show, is still catching up to the theory.

What's probably not coming anytime soon is a version of these models that never hallucinates. The architecture that generates fluent, contextually appropriate text is the same architecture that invents plausible-sounding falsehoods. You can reduce the rate. You can build systems that catch errors. You can train models to say "I don't know" more often. But the underlying tendency is structural.

---

## What This Means for You

The practical implication is simple: treat AI outputs as a first draft, not a final answer.

For creative tasks, brainstorming, and drafting, hallucinations are usually harmless and easily caught. For anything where accuracy matters — facts, citations, medical information, legal details, statistics — verify before you use it. The more specific the claim, the more skeptical you should be.

The AI isn't trying to deceive you. It doesn't know it's wrong. It's doing exactly what it was designed to do: generate the most plausible next word. The problem is that "plausible" and "true" are not the same thing, and nothing in the current architecture requires them to be.

That gap is what hallucinations are made of.

---

## References

1. [Hallucination (artificial intelligence) — Wikipedia](https://en.wikipedia.org/wiki/Hallucination_(artificial_intelligence))
2. [What are AI hallucinations? — IBM](https://www.ibm.com/think/topics/ai-hallucinations)
3. [Hallucinations: Why AI Makes Stuff Up, and What's Being Done About It — CNET](https://www.cnet.com/tech/hallucinations-why-ai-makes-stuff-up-and-whats-being-done-about-it/)
4. [What are AI hallucinations? Why AIs sometimes make things up — The Conversation](https://theconversation.com/what-are-ai-hallucinations-why-ais-sometimes-make-things-up-242896)
5. [AI Hallucinations: Why AI Makes Things Up (And What to Do) — AI Agents Kit](https://aiagentskit.com/blog/ai-hallucinations-explained/)
6. [What Are AI Hallucinations? Why Chatbots Make Things Up — CNET](https://www.cnet.com/tech/services-and-software/what-are-ai-hallucinations-why-chatbots-make-things-up-and-what-you-need-to-know/)