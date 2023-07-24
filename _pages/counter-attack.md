---
layout: page
permalink: /counter-attack/
title: counter-attack
description: Links and resources for "Computational Asymmetries in Robust Classification".
nav: false
---

For ICML 2023 attendants: you can also find me at Poster Session 4 on Thursday!

## TL; DR

Why do adversarial attacks fool adversarial defenses? It turns out that adversarial attacks
are NP-hard problems, while adversarial defenses are Sigma2P-hard (which is _much_ harder).

However, some types of defenses (i.e. inference-time defenses) are not only not affected by this asymmetry, but they can even **flip** it. For example, we introduce a very simple defense, Counter-Attack (CA), which runs an adversarial attack on the input and rejects if it the found adversarial is too close. CA is
NP-hard, but fooling a model defended by CA is Sigma2P-hard!

Finally, we study what happens when we replace exact attacks (e.g. MIPVerify) with heuristic attacks
(e.g. Carlini & Wagner, PGD...). It turns out that pools of adversarial attacks are very reliable
approximations of exact attacks, which means that they can be used by not only CA, but also potentially
other defenses.

We also release a new dataset, UG100, containing both exact and heuristic adversarial attacks computed
on a subset of the MNIST and CIFAR10 datasets (under MIT license).

## Links

[arXiv Paper](https://arxiv.org/abs/2306.14326)

[ICML 2023 poster and video presentation](https://icml.cc/virtual/2023/poster/23951)

[Medium post explaining the paper](https://medium.com/@marrosamuele/computational-asymmetries-in-robust-classification-explained-2ec475e7b3ee)

[GitHub repo](https://github.com/samuelemarro/counter-attack)

[UG100 dataset](https://zenodo.org/record/6869110)