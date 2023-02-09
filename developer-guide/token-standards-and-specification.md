---
description: >-
In this guide, we take a look at different token standards on the JuncaChain
platform.
---

# Juncachain token standards and specification

## **Overview**

In every blockchain ecosystem the token functions as the central element of a new type of economy. A token standard defines a set of rules that governs its issuance and use.

You may be familiar with ERC (Ethereum Request for Comments), which is a technical standard used for smart contracts on the Ethereum. This terminology is the origin of TRC tokens — the equivalent of ERC on JuncaChain.

### **Fungible and Non-fungible token**

There are three token standards on JuncaChain so far, each with its own unique function including JRC20, JRC21, and JRC721. These token standards can be divided into two different categories: fungible and non-fungible tokens. Fungible tokens are all equal and divisible, non-fungible tokens (NFTs) are all distinct and non-divisible.

Non-fungible tokens of the type JRC721 are those that represent a unique asset, like a certificate or a collectible in-game item. Fungible tokens are interchangeable and can be divided into smaller token units like JUNCA. The fungible token standard includes JRC20 and JRC21 as digital assets used to offer access to products and/or services on a platform. The table below compares the differences between three types of token standards on JuncaChain.\
****

|                                                 | **JRC721**        | **JRC21**                | **JRC20**     |
| ----------------------------------------------- | ----------------- | ------------------------ | ------------- |
| **Divisibility**                                | **Non-Divisible** | **Divisible**            | **Divisible** |
| **Technical requirements for Dapp integration** | **Moderate**      | **Moderate**             | **Low**       |
| **Technical requirements for exchange listing** | **N/A**           | **Moderate**             | **Low**       |
| **JuncaP Compatibility**                         | **×**             | **√**                    | **√**         |
| **JuncaX Compatibility**                         | **×**             | **√**                    | **×**         |
| **JuncaZ Compatibility**                         | **×**             | **√**                    | **×**         |
| **Transaction Fee**                             | **In JUNCA**       | **In Transaction Token** | **In JUNCA**   |

### **JRC20 - Easy integration**

JRC20 is an equivalent token standard of ERC20 built on top of the JuncaChain blockchain. JRC20 token holders would need to hold a small amount of JUNCA to cover the extremely low transaction fees.

JRC20 wrapped tokens can be easily integrated into other Dapps and get listed on exchanges. More Dapps and projects on JuncaChain are expected to be integrating and utilising JRC20 wrapped tokens in the future.


### **JRC21- Frictionless Experience**

JRC21, powered by JuncaZ Protocol, creates a frictionless experience for non-crypto users by allowing token holders to pay transaction fees by the token itself without having to hold any JUNCA in their wallet.

{% hint style="note" %}
JRC21 is under development
{% endhint %}

### **JRC721- Non-fungible token.**

Non-fungible tokens (NFTs) are all distinct and special. Every token is rare, with unique characteristics, its own metadata and special attributes. Most of the time when people think about NFT, they refer to the successful CryptoKitties game as the standard for crypto-collectibles. However, there are many other applications for JRC721 tokens. Please check our[ other guide](https://medium.com/JuncaChain/how-to-deploy-nft-tokens-on-JuncaChain-fe476a68594d) about more use-cases for NFT and the process to deploy an NFT token on JuncaChain
