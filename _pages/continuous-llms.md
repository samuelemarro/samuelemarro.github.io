layout: page
permalink: /continuous-llms/
title: continuous-llms
description: Links and resources for "Language Models Are Implicitly Continuous".
nav: false
---



# Language Models Are Implicitly Continuous

Page for the ICLR 2025 paper "Language Models Are Implicitly Continuous"

## TL;DR

LLMs are continuous models, but language is discrete. What happens when a continuous model approximates a discrete sequence? Spoiler: weird stuff.

We wanted to show that LLMs are much closer to continuous functions than we would expect. To do that, we first took existing LLMs (Llama, Gemma, Mistral, Phi…) and studied their continuous generalization: same weights, but their attention mechanism samples from a continuous signal instead of a discrete sequence.
This allows us to define bizarre concepts such as a “half-token” or a “triple-token”.

When we fed these inputs, we expected to see garbage outputs. Instead, LLMs not only understand these concepts, but also assign them a consistent semantic. For instance, four 0.75-apples are treated roughly as 4*0.75=3 apples. When varying the “size” of tokens, the predicted output varies smoothly.

Other weird results:
- LLMs have a concept of “a third of an action” or “a fourth of an event”
- LLMs can interpolate between Boolean operators, creating weird non-discrete operators
- Embedding linearity also works in the output space (!)

We don’t know why this happens, but what we do know is that thinking of LLMs as discrete models is seeing only half of the equation. To truly understand LLMs’ behavior, we need new conceptual models that take continuity into account

## Actual Abstract

Language is typically modelled with discrete sequences. However, the most successful approaches to language modelling, namely neural networks, are continuous and smooth function approximators. In this work, we show that Transformer-based language models implicitly learn to represent sentences as continuous-time functions defined over a continuous input space. This phenomenon occurs in most state-of-the-art Large Language Models (LLMs), including Llama2, Llama3, Phi3, Gemma, Gemma2, and Mistral, and suggests that LLMs reason about language in ways that fundamentally differ from humans. Our work formally extends Transformers to capture the nuances of time and space continuity in both input and output space. Our results challenge the traditional interpretation of how LLMs understand language, with several linguistic and engineering implications.

## Links

[arXiv Paper](https://arxiv.org/abs/2504.03933)

[LinkedIn Announcement](https://www.linkedin.com/feed/update/urn:li:activity:7315367010815631360/)

[Twitter Announcement](https://x.com/MarroSamuele/status/1909597362302878189)

[ICLR 2025 poster and video presentation](https://iclr.cc/virtual/2025/poster/29613)

[GitHub repo](https://github.com/samuelemarro/continuous-llm-experiments)
