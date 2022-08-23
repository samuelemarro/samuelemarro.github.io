---
layout: post
title: Revisiting GreenNFT, one year later
date: 2022-08-15 11:12:00-0400
description: Has the model held up?
tags: #blockchain environment
categories: blockchain 
---

# TL;DR

- Our predictions were pretty correct
- Still, the data are outdated: emissions-per-dollar have decreased by ~40%
- The model diverges significantly from de Vries', although there are several factors that can explain it

# Introduction

In May 2021, [Luca Donno](https://twitter.com/lucadonnoh) and I published "Green NFTs: A Study on the Environmental Impact of Cryptoart Technologies", a 16-page investigation into the actual greenhouse gas impact of NFTs. While at the time the impact of blockchains was already well studied (see [de Vries' landmark paper](https://www.sciencedirect.com/science/article/pii/S2542435118301776) on the topic), whether the NFT market played a role in emissions was still up for debate, with some even claiming that [NFTs have](https://medium.com/superrare/no-cryptoartists-arent-harming-the-planet-43182f72fc61) [no impact](https://www.artnews.com/art-news/news/nft-carbon-environmental-impact-1234589742/).

According to our model, that couldn't have been farther from the truth: spending 1 dollar on a transaction (either by minting an NFT, bidding or transferring it) causes an estimated emission of 1.305 kgCO2eq. To put that number in context, a typical NFT (using May 2021 data) would have emitted as much as driving 1862 km/1157 mi.

Our work (the paper + the companion website, now out of date) won a 3k USD award and is now cited by [Wikipedia's article on NFTs](https://en.wikipedia.org/wiki/Non-fungible_token) (which in my opinion still represents my greatest academic achievement).

The thing is, time flies very fast in the blockchain space, to the point where we were pretty sure that our results wouldn't have been relevant for more than a couple months. Is this what actually happened?

# Predictions, Predictions

The paper made four medium-term predictions:
- [https://eips.ethereum.org/EIPS/eip-1559](EIP-1559), a new fee auction mechanism, would have reduced miners' revenue (and thus global Ethereum emissions) by 30%
- EIP-1559 would have reduced miner's revenue from transactions by 98%
- Layer-2 blockchains, which are significantly less polluting than Ethereum, would have been much more common
- The Merge, i.e. the transition of Ethereum to Proof-of-Stake (which we expected to happen in January 2022) would have made Ethereum emissions negligible

Most of them turned out to be fairly correct: the revenue (in ETH) of miners decreased by 30-40%, while the revenue (in ETH) from transactions decreased by 90% (note that the ETH price crash led to the USD revenue being even lower). Moreover, L2s went from a niche topic to a common tool for low-fee transactions, with Optimism and Arbitrum becoming well-known names and a growing interest in zero-knowledge rollups.

# What Didn't We Predict?

May 2021 was a very different time: ETH/USD was inching ever closer to 4k, gas prices fluctuated between 50 and 150 Gwei, and "En Eff Tees" were just starting to leak into mainstream knowledge. While we did expect price fluctuations to affect miners' revenue, there were four events that made our data outdated:
- September 2021: the People's Bank of China forbids all Chinese citizens from using cryptocurrency. Most Chinese miners were faced with three choices: closing shop, moving to other countries, or continuing their operations underground
- February 24th, 2022: Russia invades Ukraine. Natural gas prices skyrocket, followed by increases in the price of electricity as well. Mining suddenly becomes more expensive in many regions
- April 2022: Tim Beiko (Ethereum Foundation) announces a [new delay of the Merge](https://twitter.com/timbeiko/status/1513610106721603587) to September
- May 2022: the $LUNA + $UST crash worsens an already strong bear run. The gas price, which was already declining, reaches in a couple months an average below 20 Gwei

# The New Data

New circumstances call for new sources of data.

First, the original paper used Silva's region estimate from April 2019, which is now more than two years ago. We therefore turned to mining pool statistics, which, despite their flaws (mainly lack of region reporting and no guarantee that a miner using a node from a certain region is actually in that region), still represent the best source on the geographical distribution of miners. Instead of the older AMD RX 590, we picked as reference GPU the RTX 3060 Ti. We also used updated electricity prices, although the fact that some countries report average prices at the end of the year means that the electricity price is likely underestimated (which means that the emissions are probably overestimated).

You can find all our (August 1st) data and calculations [here](https://docs.google.com/spreadsheets/d/18MDF_jAqI217GSUl90akfEp7cCi1gRg5eStPoLwUKXw/edit?usp=sharing).

Running the model on the new data gives us the following results:
- A miner earning 1 USD corresponds to emitting 0.791 kgCO2eq (-40%)
- Only ~10% of a transaction fee goes to miners, which means that the individual impact of spending 1 USD on transaction fees is ~0.08 kgCO2eq (note that this doesn't take into account the effects that transactions have on prices)
- The global impact of the Ethereum blockchain is 5.88 MtCO2eq per year

The last figure is significantly different from [de Vries' current estimate of 46.72 MtCO2eq(https://digiconomist.net/ethereum-energy-consumption), which is based on a variant of his [Bitcoin model](https://digiconomist.net/bitcoin-energy-consumption). What can explain such a difference?

- De Vries uses a fixed electricity price of 0.05 USD/kWh, which doesn't take into account the recent increase in prices (our weighted average is 0.16 USD/kWh). The price is then doubled to 0.10 USD/kWh to reflect the lower efficiency of the Ethereum hashing algorithm; however, this fixed correction fails to capture the efficiency improvements over the years
- De Vries doesn't provide information on how he estimates the geographical distribution of miners; it is reasonable to assume that, similarly to his Bitcoin model, he relies on [Cambridge's Bitcoin estimates](https://ccaf.io/cbeci/mining_map), which might not correspond to the distribution of Ethereum miners
- For Bitcoin, de Vries assumes that electricity represents 99.5% of total costs for miners, compared to our ~90% figure (which we derived from our estimates of the cost of both hardware and electrictity)

# New Predictions

In the spirit of the original paper, here are four new (and more cautious) predictions:
- The impact of Ethereum will increase again, as the price of ETH rises again and the price of electricity stabilizes
- The Merge will happen within 6 months: the merging of the test nets has been completed successfully, and while I'm still a bit skeptical that the Merge won't be delayed in September, I'm confident that the team behind it can pull it off in less than 6 months
- If the Merge actually succeds, Ethereum emissions will decrease by at least 99%, since the entire concept of electricity-intensive computation will be abandoned
- If the Merge will be delayed again, it will provide a boost to the growth of L2s, which means that at the very least a growing percentage of transactions within the Ethereum ecosystem will be on energy-efficient blockchains

For now, we can only hope that the Merge will go smoothly as planned, and that it will bring Ethereum emissions to such low levels that the entire discussion regarding its impact (including our paper) will become irrelevant. And, to be fair, that's not a bad reason for a work to be forgotten.