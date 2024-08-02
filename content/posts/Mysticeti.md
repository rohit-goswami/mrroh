---
title: "Demystifying Sui's Mysticeti"
date: 2024-08-02
draft: false
author: mr-unomi
cover:
  image: img/image.png
  alt: "Cover Image for Demystifying Sui's Mysticeti"
  caption: "Cover image for the article on Sui's Mysticeti"
tags: ["Sui", "Consensus"]
categories: ["tech", "Web3"]
description: "Let's understand Sui's groundbreaking innovation Mysticeti"
---

# Mysticeti: The Breakthrough Consensus Protocol Now on Sui Mainnet
Recently Sui Foundation has launched Mysticeti on Mainnet, the groundbreaking consensus protocol that has reduced consensus time on Testnet by a staggering 80 percent!


![](https://imgur.com/uRZCDzl.png)

Mysticeti achieves the fastest overarching latency, reaching consensus on Testnet in just 390ms with 640ms settlement finality. This is an 80% reduction from the previous Narwhal-Bullshark protocol 

The Sui blockchain, already known for its impressive transaction speeds, is set to get even faster with the introduction of a new Byzantine consensus protocol called Mysticeti. This innovative protocol drastically reduces transaction latency and lowers CPU requirements for validators, making it a game-changer in web3. 

## Understanding Mysticeti

Mysticeti is a consensus protocol based on a Directed Acyclic Graph (DAG) structure. In simple terms, it’s a way to organize and process transactions in a network, ensuring that all participating nodes agree on the transaction history without delays. This protocol is specifically designed to handle Byzantine faults, which are failures where nodes may act maliciously or unpredictably.

![](https://imgur.com/xe3yUHB.png)

## The Challenges of Traditional Consensus Protocols
Traditional blockchain protocols like PBFT (Practical Byzantine Fault Tolerance) and the previously used Narwhal-Bullshark in Sui, often struggle with high latency and resource consumption. For example, Narwhal-Bullshark required multiple round trips to certify each block, leading to delays. These protocols also needed validators to perform extensive cryptographic operations, consuming significant CPU resources.

In 2023, Sui set a record with 5,414 transactions per second (TPS) and 65 million transactions in a single day, using the Narwhal-Bullshark algorithm. While this was impressive, the need for even lower latency and higher efficiency was clear.

## How Mysticeti Innovates
Mysticeti addresses the limitations of traditional consensus protocols in several ways:

#### Eliminating Explicit Certification:
Unlike Narwhal, which required validators to certify each block individually, Mysticeti allows blocks to be shared and committed without explicit certification. Validators simply sign and share their blocks, reducing the need for multiple round trips and cutting the latency by 50%. This is achieved by using a novel commit rule that processes blocks in half a round trip, the lowest known for any DAG-based consensus.

#### Efficient Block Processing:
Mysticeti removes the primary-worker distinction seen in Narwhal, where each validator had a primary and worker nodes. This distinction sometimes led to additional delays as workers had to wait for a primary to share a block. Mysticeti inlines transactions directly within the primary’s block, allowing for high throughput of over 100,000 TPS without the need for additional processing rounds.

#### Faster Commitment Rules:
While Bullshark committed blocks every two or three rounds, Mysticeti appoints multiple leaders each round. This approach allows more transactions to be committed earlier, significantly reducing the time users wait for transaction confirmations.

![](https://imgur.com/f5onTSH.png)

## Real-World Performance
Mysticeti has demonstrated exceptional performance in testing environments. It can commit transactions within three message delays, equating to about 500 milliseconds on a wide area network (WAN). This is a significant improvement over traditional protocols, which often require much longer. Furthermore, Mysticeti maintains a solid throughput of over 50,000 TPS under low latency conditions and can even reach over 100,000 TPS when latency is around 1 second.

## Fast Path for Owned Object Transactions
Sui's design includes a fast path for transactions involving single-owner objects, like digital assets owned by a single entity. Mysticeti supports this fast path by allowing validators to vote on these transactions directly through the DAG, without needing additional signatures. This method achieves finality in just two round trips, approximately 250 milliseconds on a WAN. The fast path is especially efficient, requiring less CPU power and enabling five to ten times faster transaction processing.

Mysticeti's launch on the Sui Mainnet is a game-changer, opening doors to numerous innovations in blockchain technology. With drastically reduced latency and high throughput, Mysticeti sets a new standard for consensus protocols, making real-time applications, enhanced DeFi services, and scalable blockchain solutions possible.

see ya! until next time :)