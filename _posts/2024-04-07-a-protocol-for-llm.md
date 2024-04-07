---
layout: post
title: A Protocol Sketch For LLM Communication
date: 2024-04-07 09:12:00-0400
description: So, how do we make LLMs talk to each other?
tags: #ai
categories: ai
---

# TL;DR

- Natural language is flexible, structured data is efficient
- A protocol that supports structured data but allows natural language as a default achieves the best of both worlds
- Machines can use documents to describe highly specific communication protocols, which can be handled by traditional code routines
- For everything else (including negotiating new protocols, handling code failures and writing new routines) we can use LLMs

# Introduction

About 1.5 years ago I made a blog post where I stated that natural language was the natural choice to allow flexible, programmer-less interfacing between machines. In that time span, a lot happened! GPT-4 was released and made a lot of people question their definitions of general intelligence. Llama and Mistral showed that good models aren't necessarily closed source. And, most importantly, there has been a decisive trend towards bigger, more expensive and more centralized models, although the advances in quantization and MoE have somewhat slowed the growth.

At the moment of writing, all LLMs that have comparable performance to the "big guys" (ChatGPT, Claude, Bard) are 70B models, which have requirements way beyond those the average consumer can afford. For example, [Qwen-1.5-72B](https://qwenlm.github.io/blog/qwen1.5/) (#9 in the [Chatbot Arena](https://chat.lmsys.org/)) requires, even with quantization, [two NVIDIA A100 80 GB](https://qwen.readthedocs.io/en/latest/benchmark/hf_infer.html), which puts a the cost of running Qwen locally at about 40k USD.

So, how can we make open source LLMs more available to the general public?

I think that the best tool to compete with massive LLMs is to use networks of (relatively) small LLMs. Having 10, 100 or 1000 LLMs talking to each other is definitely less efficient compared to just training a model that is 10, 100 or 1000 times larger, but it also means that you don't need an entire data center just to do inference.


And so, while I'm here in Oxford for my research visit, I thought it would be a good chance to expand upon the idea further, with the goal of building a concrete first step towards networks of LLMs. And I'd argue that the first step towards a network is how nodes in such a network communicate.
Specifically, what I'm interested in is a simple protocol to allow communication between (LLM-powered) machines. Rather than directly outline the protocol I have in mind, I'll walk you through the reasoning that led me to this specific implementation.

# A Sample Task

We're going to use a very simple task: querying the price of a stock. There are two machines, Alice and Bob, each having their own databases, with their own schemas and their own conventions:

[!](https://i.imgur.com/zuFelp4.png)

Alice wants to query Bob to obtain the current price of MSFT, which at the time of writing sits at 425.52 USD.

We have two seemingly competing goals:
- Flexibility: the machines should be able to adapt to changes in data, schema or goals with little effort;
- Efficiency: performing a task should be as fast and low-resource as possible, especially if it is performed multiple times.


# Level 0: Manual Implementation

The most standard approach for answering a query is for someone to expose an API for Bob, while someone else writes some code for Alice to query the API, convert it into a database-friendly schema and store it.

This is as efficient as it gets: there's a reason if pretty much all of machine-to-machine communication is performed over APIs. But it's not flexible: if you want to use a different API, or if you want to query more information (e.g. the price-to-earnings ratio), or even if you just want to change the internal schema of your database, you need a human to implement the changes.

# Level 1: Natural Language

But it's 2024, and LLMs are hot. Since LLMs can solve everything, let's just build two LLMs that chat with each other!

Specifically, we add two LLMs that can interface with Alice's and Bob's databases, respectively. Both LLMs are capable of using natural language to communicate with each other:

[!](https://i.imgur.com/rc26z8X.png)

This is as flexible as it gets: if Bob's database changes schema, Bob only needs to be informed of the new schema, while from the point of view of Alice nothing changed. If Alice wants to query the price-to-earnings ratio, it's just a matter of changing the question. If Bob's server goes down, Alice can start querying another natural language-friendly machine without any major disruptions.

At the same time, however, we are using two language models to send a floating point value between two databases. This is the computational equivalent of shooting a fly with a bazooka.

# Level 2: Reinventing APIs

Wouldn't it be just easier if we could have the LLMs agree on a way to send the data? For example, suppose that Alice's query is:

```
Alice: What's the price of MSFT? Send the data as a JSON with a single field, "price", containing the price of the answer
Bob: { "price" : 425.52 }
```

If Bob is sufficiently consistent with its replies, Alice can just write a simple routine to store the data:
```
const result = await Alice.queryPrice('MSFT')
const price = JSON.parse(result).price
await myDatabase.update({ 'MSFT' : price })
```

This means that we don't need to use Alice to store the result in the database: we can rely on good ol' code, written by the LLM. Similarly, Bob can ask Alice to provide queries in a standard format, so that it can also use routines. So, the first communication might be something like:

```
Alice: I want to establish a protocol between us. I want to know the price of a stock.
Bob: I can send you the price as an XML, something like <xml>The price of <stockName>MSFT</stockName> is <stockPrice>425.52</stockPrice></xml>
Alice: Can we use JSON instead?
Bob: Ok, how about you send me a JSON with the price query, e.g. { "priceQuery" : "MSFT" }, and I send you a JSON with the result, e.g. { "price" : 425.52 } ?
Alice: Sounds good!
```

After this natural-language negotiation, future communication might be something much more terse:

```
Alice: { "priceQuery" : "MSFT" }
Bob: { "price" : 425.52 }
```

In this case, Alice and Bob could just use the routines written by the LLMs, which means that the latter don't need to be invoked.

In order to ensure that Alice and Bob are both sure that they're using the same protocol, we might add an identifier for the protocol, plus an ACK/NAK token to programmatically check if the other party understood which protocol they're using:

Alice: ALICE-BOB-STOCK-PRICING { "priceQuery" : "MSFT" }
Bob: ACK { "price" : 425.52 }

Congratulations, we've just reinvented APIs. However, this approach has several benefits over a regular API:
- If either Alice or Bob need to change the protocol, they can just negotiate a new one
- If Alice needs to make some unusual queries, it can just ask in natural language

In general, the cool thing about this approach is that natural language is a universally supported default for communication. If there's a more efficient protocol, that's great! But if there are issues or unexpected changes, natural language is still an option.

[!](https://i.imgur.com/b2wfxqF.png)

# Level 3: Scaling to a Network

Our protocol is perfectly fine for communication between two machines. But when we scale to a network of machines, there are three new problems:
- Negotiating a protocol with each machine in the network is inefficient
- If a query is forwarded to a different machine, it might lack the context required to understand the query
- There might be two protocols with the same name, or a protocol with two different names

Fortunately, we humans have created a tool to efficiently tell the rest of the world how to implement a protocol: we call them standards! Whether they're RFCs, ISOs, ERCs or whatever, these documents provide everything two parties need to initiate a communication.

So, for our stock market info, we can just write a document:
```
The client sends a plaintext JSON document having a single field, "priceQuery", which is the ticker of the stock (in uppercase). The server replies by sending a JSON document having a single field, "price", which is the price (in USD) of the corresponding stock. This price is expressed as a floating-point value.
```

Note that:
- Unlike a schema, this document also provides the semantics for the data, which means that a model that has never seen this document before can still understand the data that was received;
- The document can range from general-purpose communication formats to highly specialized data formats;
- LLMs can write these documents.

Now we just need a way to assign a unique identifier to this document. The classic approach would be to rely on a standards agency to assign codes, but:
a. That defeats the whole point of having a decentralized system;
b. No agency (even with the help of LLMs) could process and catalogue all the possible communication protocols that two LLMs might establish.

There is, however, an alternative, inspired by the [IPFS Protocol](https://ipfs.tech/): using the hash of the document as a unique identifier. By definition, a document can only have one hash, and with a good enough hashing scheme the probability of collisions is so low (e.g. 1 over 2^160, if we're using SHA1) that they might as well be non-existant.

And that's it! The final communication, assuming that we're using Base64-encoded SHA1 as hashing protocol, is thus something like this:

```
Alice: o2R8vS9V7BqfhHQFVWapyCVyqNs= { "priceQuery" : "MSFT" }
Bob: ACK { "price" : 425.52 }
```

All of this happens without the intervention of the LLMs: if, however, there are any issues with the communication, the LLMs can simply intervene. For example, in case Bob doesn't have a routine to handle the protocol, it can just send a NAK, which makes Alice send the corresponding protocol (which is then read by Bob's LLM). Similarly, if for some reason the routine fails, the LLM can take over. As a flowchart:

[!](https://i.imgur.com/fNvqXCD.png)

# Conclusion

This pretty much covers the core of my idea for the communication protocol. Of course, this is by far not a full-fledged proposal, as there are lots of open questions, such as:
- How do we broadcast information about protocols to the rest of the network?
- How can a machine trust a previously-unknown protocol to be safe?
- How can we implement some common features of other communication protocols (e.g. authentication, retrying, message forwarding...)?
- Is there a preferrable way to write a protocol document?
- Which hashing algorithm should be used?

I hope to cover potential solutions to these challenges in the next posts. That said, if you have any opinions on the protocol, its strengths and its flaws, feel free to reach out! The more I think about LLM communication, the more it looks like building a proper ecosystem (protocols, software, know-how...) for networks of LLMs is something that will require the effort of an entire community. Fortunately, it looks like quite a lot of people seem to care about keeping Machine Learning decentralized.