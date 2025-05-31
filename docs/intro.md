---
sidebar_position: 1
slug: /
---


# Introduction

The Simple Ledger Protocol (SLP) is a protocol for creating tokens on UTXO blockchains, such as Bitcoin Cash (BCH) or eCash (XEC). This site provides documentation and resources for working with these tokens, and focuses on the Bitcoin Cash blockchain.

## What are Tokens?

In the context of blockchain, tokens are digital assets that represent value or utility within a specific ecosystem or application. They are created and managed on a blockchain, leveraging its security and decentralized nature. There are many use cases for tokens, including representing stocks, securities, registries, smart properties, utility tokens, contracts, coupons, bonds, demand deposits, local currencies, physical assets, or simply storing data.

Tokens can generally be categorized based on their fungibility:

### Fungible vs Non-fungible (NFT) Tokens

*   **Fungible Tokens:** These are tokens where each unit is interchangeable with another unit of the same type. For example, one Bitcoin Cash satoshi is interchangeable with any other Bitcoin Cash satoshi. Similarly, if you have 100 units of a specific fungible SLP token, any one of those units is identical to another. SLP supports the creation and transfer of fungible tokens, often referred to as [Type 1](https://github.com/simpleledger/slp-specifications/blob/master/slp-token-type-1.md) tokens. The GENESIS transaction creates a new fungible token, MINT transactions add to the supply, and SEND transactions transfer ownership.

*   **Non-fungible Tokens (NFTs):** In contrast, non-fungible tokens are unique and not interchangeable. Each NFT represents a specific, distinct asset, even if it's part of a collection. NFTs are used to represent non-fungible assets on the blockchain. The Simple Ledger Protocol supports NFTs through the [NFT1 specification](https://github.com/simpleledger/slp-specifications/blob/master/slp-nft-1.md). The workflow for creating NFTs using SLP is different from protocols like Ethereum's ERC-721.

It's worth noting that Type 1 Fungible tokens can also function as an NFT if the quantity produced is 1, the minting baton is burned, and the decimal precision is 0 (no fractions of tokens are allowed).

Both types of tokens can be created and managed using the command-line interface (CLI) application [psf-slp-wallet](https://github.com/Permissionless-Software-Foundation/psf-slp-wallet).

## How SLP is Different from Other Token Protocols

The design philosophy behind SLP was heavily influenced by the experiences and challenges faced by earlier token protocols on other blockchains. Several key requirements shaped the SLP protocol:
*   **Permissionless:** Token issuance and transfer should not require permission.
*   **Simple:** The system should be easily understandable and straightforward.
*   **Robust:** A minimal, unambiguous rule set supports a fault-tolerant consensus layer.
*   **Non-invasive:** It should require no changes to the underlying blockchain protocol.
*   **Extensible:** The system should allow for future versions of tokens.

Comparing SLP to other token schemes highlights its pragmatic value:

*   **Counterparty (on Bitcoin BTC):** One of the oldest protocols. Faced hostility from Bitcoin developers, leading to rule changes that discouraged token use (the "OP_RETURN wars"). The lesson here is that a token protocol relying on the good will of the core blockchain developers can be at risk if that relationship sours.
*   **Ethereum (ERC20):** While successful in popularizing tokens (like during the 2017 ICO boom), the high transaction fees on the Ethereum main chain make token usage prohibitively expensive for many businesses. The lesson is that high fees significantly reduce the economic viability of a token program.
*   **Wormhole (on Bitcoin Cash):** This protocol was based on a fork of the ABC full node software. When the Wormhole development team lost funding, they could no longer keep their node software updated with the regular Bitcoin Cash network upgrades. This tight coupling to a specific full node implementation led to the Wormhole token ecosystem suddenly dying when its node was forked off the network. This demonstrated that **tight coupling to a specific full node is an anti-pattern** to be avoided.
*   **CashToken (on Bitcoin Cash):** Another protocol on BCH that is tightly coupled to the full node. Building on CashToken means a business is 'locked in' to the Bitcoin Cash blockchain, increasing platform risk because the protocol has not been adopted by other blockchains.

SLP addresses the issues seen in these previous attempts by being **loosely coupled** to the full node software. The full node simply processes SLP transactions as standard Bitcoin Cash transactions with OP_RETURN data; it doesn't care or know anything specific about the token functionality. Token transactions are validated by a separate indexer, rather than the full node itself. This loose coupling is a key reason SLP can be adapted to any blockchain that includes a text field in its transactions, including Bitcoin forks like Bitcoin Cash (BCH), eCash (XEC), Nexa, and Radiant, as well as non-Bitcoin forks such as the AVAX X-chain or Qortal. There are currently mature SLP ecosystems on both the Bitcoin Cash and eCash blockchains.

Another distinguishing feature is the **SLP validation model**. The validity judgment of every SLP transaction is strictly determined by its own contents, the contents and validity judgments of its input transactions, forming a directed acyclic graph (DAG). This allows light wallets to construct DAG proofs for transactions. Importantly, SLP validity is **completely independent of block-chain confirmations or even double spending**. Any SLP validation judgment is therefore permanent because the SLP consensus rules are fixed.

While miner-validated tokens (like CashTokens) can prevent accidental burns by rejecting malformed transactions, unintentional burning of SLP tokens *is* possible if outputs are improperly spent. However, the infrastructure surrounding SLP is extremely mature. Tools like the [minimal-slp-wallet library](https://www.npmjs.com/package/minimal-slp-wallet) and the open source web wallet found at [wallet.psfoundation.info](https://wallet.psfoundation.info) have been in use for years without issue regarding burning tokens. Furthermore, businesses can implement a 'burning policy' to detect and compensate users if accidental burns occur.

SLP also offers **scalability** options, particularly through the use of indexers like the [psf-slp-indexer](https://github.com/Permissionless-Software-Foundation/psf-slp-indexer). There's no requirement for every indexer operator to track every single token. The psf-slp-indexer supports a blacklist, allowing operators to ignore problematic tokens, thus reducing computational burden.

## SLP Address Format

To mitigate the risk of users accidentally spending a UTXO that holds a token balance in a non-token-aware wallet, SLP utilizes a distinct address format. This format uses the same encoding scheme as the standard Bitcoin Cash CashAddr format, but with a different prefix: **"simpleledger:"** instead of "bitcoincash:". Because the prefix is part of the checksum, an SLP Addr is considered an invalid encoding for applications expecting a CashAddr, and vice-versa. SLP wallets can handle this temporary incompatibility by providing an [address converter](https://address.fullstack.cash). Example of a SLP Addresses: `simpleledger:qqmtw4c35mpv5rcjnnsrskpxvzajyq3f9ygldn8fj0`.




## Conclusion

The Simple Ledger Protocol provides a simple, non-invasive, and robust framework for creating and managing both fungible and non-fungible tokens on the Bitcoin Cash blockchain. By leveraging the OP_RETURN field and adhering to a clear set of rules, SLP enables a rich token ecosystem while remaining loosely coupled from the core BCH protocol, avoiding the pitfalls faced by other token schemes. Developers have access to a variety of tools and libraries to build applications that support SLP tokens.



