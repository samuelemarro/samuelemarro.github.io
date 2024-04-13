---
layout: post
title: Networks of Neural Networks
date: 2022-08-22 11:12:00-0400
description: Natural language models as a tool for decentralized computation 
tags: #ai
categories: ai
---


# TL; DR

- Natural language is a great universal language for machine-readable information
- We can create natural language models that formulate questions and talk with other AIs using natural language
- Such a network-of-neural-networks would provide a decentralized alternative to the more typical "supercomputer AI"

# Introduction

Humanity has a lot of information and computational power, and both have long proven to be very useful.
It is thus natural that entities that have the capacity to both collect information on large scales and process it in meaningful ways tend to have an advantage over those that don't. At the same time, this advantage tends to benefit organizations that already have considerable resources, creating a potentially dangerous feedback loop.

That said, it's not like computational power is inaccessible. The most powerful supercomputer in the world is the [Hewlett Packard Enterprise Frontier](https://en.wikipedia.org/wiki/Frontier_(supercomputer)), with roughly 1.1 exaFLOPS, corresponding to the computational power of ~1M smartphones. An impressive figure, but it also means that the citizens of [Phoenix, Arizona](https://en.wikipedia.org/wiki/Phoenix,_Arizona) could theoretically outperform a supercomputer by downloading an app. Similarly, most information is either publicly available (e.g. Wikipedia) or being produced by regular users.

A more relevant asymmetry concerns the _programming power_: a government or a large company can afford to pay programmers and employees to collect and process information in a centralized manner.
Open-source and grassroot projects tend, on the other hand, to be fragmented into countless independent systems, each with its own objective, approach and policy. In my opinion, that's a feature, not a bug.

The downside is that a large portion of information and computational power is thus locked away: twenty NGOs could collect data on the population of trouts in different parts of the world, but unless someone manually collects the data from all their websites and standardizes them, you can't run queries on the sum of their knowledge. They could also pool their computational resources to create a ML model of which factors influence the trout population, but convincing twenty organizations to spend money to setup such a system is going to be difficult at best.

Is there thus a way to pool information and computational resources without sacrificing decentralization?
Let's consider two existing techniques that allow machines to share machine-readable information, and why they fall short of the task.

# Ontologies

In short, ontologies are standardized machine-readable descriptions of entities that are related to other entities. For example, let's say that you want to formalize "Tim Berners-Lee was born in London". To do so, you write something like this:
```
_:a <https://www.w3.org/1999/02/22-rdf-syntax-ns#type> <https://schema.org/Person> .
_:a <https://schema.org/name> "Tim Berners-Lee" .
_:a <https://schema.org/birthPlace> <https://www.wikidata.org/entity/Q80> .
<https://www.wikidata.org/entity/Q80> <https://schema.org/itemtype> <https://schema.org/Place> .
<https://www.wikidata.org/entity/Q80> <https://schema.org/name> "London" .
```

Here, "_" represents Tim Berners-Lee, while Q80 is WikiData's ID for London. As you can see, everything is standardized and machine-readable. Even the relations between entities (i.e. name and birthPlace) are standardized, and these standards are accessible in machine-readable formats. There are standards for pretty much everything, from books to people to ideas. They can be used by servers and people alike to to share information in an universal manner.

Ontologies suffer from two problems:
- They don't capture the actual meaning of concepts: a machine can easily read the birthPlace schema, but it cannot dentify relations between concepts (e.g. birth and death) or use the collected data in other contexts, unless told so by a programmer
- They are standardized: if your goal is to formalize the entirety of human knowledge, any type of standard is going to be either too formal and complex, or too loose and devoid of nuance

In practice, this makes working with ontologies both frustrating and mind-numbing.

# API Hell

APIs represent the most common way to allow machines to communicate between each other.
We can split the API usage process in four parts:
- Figure out what the various API paths and arguments do
- Figure out how the data offered by the API can be used to obtain the information you want
- Write the code to pull data
- Write the code to convert the received data into something usable for your program

The process of writing an API is similar:
- Figure out what information you have
- Figure out how you can expose information as standardized data
- Write the code to convert your program's internal representation of information into something standardized
- Write the code to provide data

APIs are the basic tools of machine-to-machine communication, and yet both exposing information and retrieving it involve considerable work. Ontologies suffer from the same problem, with the only difference being that you're exposing XML/JSON documents instead.

This type of work can be well achievable by motivated programmers, but can we say the same about regular folks? Would we be able to convince a rural doctor to provide medical statistics (e.g. to track the spread of diseases) _and_ implement a standardized API _and_ input the data in a standardized format?

# The Universal Language

If we want to create a communication system that allows machines to gather and share information, we need to follow some ground rules:
1. It must be flexible enough to capture concepts with both high and low levels of nuance
2. Machines interacting with it must have some way to connect concepts beyond what is said in the document
3. The system must not be difficult to learn or use

There's actually a language that (kinda) satisfies all three requirements: natural language.

While points 1 and 3 are both self-evident, it is only in the recent years that natural language models have been able to reason beyond the immediate data. If you ask GPT-3 what's a "colorful animal with wings that is capable of singing", it will (on a good day) say that it's a bird.
Combined with recent advances in neural programming, it suggests a way for organizations to pool data and information without any effort: have natural language models act as "middlemen", with natural language being a flexible (if ambiguous) API.

# Neural Crawlers

Imagine a Neural Network (or whatever ML model we will come up with) that is capable of performing three tasks:
- Reading natural language documents (including web pages) and tabular data
- Coming up with relevant Google queries and questions
- Chatbot-level conversation

All of these tasks have been studied intensively, although there's little work on unifying them in a more general model.
Combined with existing search engines, crawling tools and translation software, we can create a Neural Crawler (NC), which works as follows:
- The user asks a question in natural language (e.g. "What's the average weight of a male trout in August 2022?")
- The NC comes up with relevant queries ("fish weight database", "common trout locations", "north dakota trout data"...)
- The NC crawls the found websites, parsing the data whether it's in an Excel file, a PDF document or a web page
- The NC pools the information and provides a final answer, in addition to a list of sources so that the user can check its correctness

Google already offers a similar mechanism: if you try to search a question, it will attempt to provide an answer, although it is usually only taken from one source.

While a fully-supported Q&A system would represent a massive improvement in user experience (and make all my Google-Fu skills outdated), the true benefit comes from allowing other AIs to interface with this system. A model with a natural language module would be able to access the entirety of humanity's knowledge without the massive data and hardware requirements that general-purpose models currently face. Such "Google-Fu" models would then be able to make contributions by processing information in interesting ways or by combining private data (without exposing it).


# The World Wide Web of AIs

The next logical step is to create a network of such models. If a model doesn't have enough information, it can just query other models, which can then query other models and so on. Each AI would be able to combine sources, add new information or provide unexpected insight. Since the "interface" uses natural language, there's not even a need to define a new standard: even regular emails would be a valid way to communicate between AIs (although there would probably be a slow transition towards a semi-defined standard, for example to simplify chatting). Note that this removes the need of relying on a search engine.

It's possible that this system would need financial incentives in order to encourage meaningful contributions. For example, the models might agree on a price in exchange for the information or the computational power. Smart-contract blockchains seem to be the obvious decentralized choice for signing these agreements, although they would probably need to process transactions in the order of the billions to handle high-speed communication between NNs. In order to filter out malicious or inaccurate sources, there could even be a web-of-trust system like in PGP.

This network of neural networks would be under all points of view a macro-AI, capable of acting like a large-scale AI (although less efficiently) while still being decentralized. In a world where general models require millions of dollars to be trained and hosted, this represents an alternative way to reap the benefits of AI without giving up independence.

Of course, there are several obstacles to an actual implementation of this system, such as:
- ML models are still fragile and tend to fail when dealing with unexpected data
- ML models struggle to understand if data is reliable, and can easily be misled (see: the entirety of the adversarial ML field)
- Natural language models are still way too big (or, conversely, hardware is not powerful enough yet) to conceive running a good model on consumer hardware
- There's no guarantee that people would agree on some form of payment protocol

However, I believe that, with enough dedicated effort, it could be achieved without major leaps in AI or hardware technology.

# Conclusion

[Memex](https://en.wikipedia.org/wiki/Memex), [Xanadu](https://en.wikipedia.org/wiki/Project_Xanadu) and the [World Wide Web](https://en.wikipedia.org/wiki/World_Wide_Web) all shared the same objective: simplifying access to information. However, AI-driven information is becoming more expensive to produce, and at the same time more powerful in the hands of those who have it. Even if Neural Crawlers and the Network of Neural Networks don't turn out to be the solution, democratizing information is a key challenge for the 21st century, one which I hope will be solved by individuals motivated by the same ideals that moved the founders of the Internet.