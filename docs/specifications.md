---
sidebar_position: 4
---

# Specifications

## Overview of Simple Ledger Protocol

Simple Ledger Protocol (SLP) is a system designed to enable both *fungible* and *non-fungible* (NFT) tokens. SLP offers a solution that is both simple in terms of its implementation. Because the protocol is so simple, it can be ported to almost any blockchain, preventing lock-in and platform risk to businesses using these tokens.

SLP achieves token functionality by leveraging the existing features of Bitcoin Cash transactions. It utilizes the metadata stored within the **OP_RETURN** field of standard Bitcoin Cash transactions for the issuance and transfer of tokens. Any blockchain that provides an arbitrary data field in transactions (similar to OP_RETURN), can implement the Simple Ledger Protocol for tokens.

Consensus on the interpretation of the OP_RETURN data is achieved through token users and market participants adhering to a prescribed set of simple rules. The protocol defines **four types of token transactions** contained within the OP_RETURN metadata:
1.  **GENESIS:** Defines and issues a new token type.
2.  **MINT:** Allows for the creation of additional tokens for an existing token type, provided the holder controls the minting baton.
3.  **SEND:** Facilitates the transfer of tokens between users. This is the most common transaction type.

It is important to note that you **cannot send multiple types of SLP tokens in the same transaction**. Also, tokens can be lost if a token-containing output is improperly spent.

### Overview for creating NFTs

*Note: this section should be moved to its own chapter*

The process for creating SLP NFTs involves two main steps according to the NFT1 specification:
1.  **Create an NFT1 group minting token:** This is a new SLP token specifically created for group minting. The token ID of this group token serves as the **Group ID** for all the NFT children that will be created under it. It is recommended that the decimal precision for the NFT group token is set to 0, although this is not strictly required. NFT1 group minting tokens use **Token Type 0x81** in all their GENESIS, MINT, and SEND transactions.
2.  **Create individual NFT child tokens:** New SLP tokens representing the individual NFTs are created by spending a quantity greater than 0 of the NFT1 group minting tokens in a new GENESIS transaction. It is recommended that the wallet automatically breaks the Genesis output quantity into multiple outputs, ideally with a value of 1, for outputs intended to create NFT children. NFT1 child tokens have specific requirements:
    *   They use **Token Type 0x41** in all their GENESIS and SEND transactions.
    *   The GENESIS transaction *must* include spending a valid NFT1 parent token (quantity > 0) at transaction input index 0.
    *   The GENESIS quantity *shall* be set to 1.
    *   They *shall not* have a baton vout (baton vout must be set to 0x4c00 in NFT1 child GENESIS).
    *   They *shall have* 0 decimal places (decimals must be set to 0x00 in NFT1 child GENESIS).

Wallets that handle NFT1 tokens must ensure they are spent using the correct Token Type field (0x81 for group, 0x41 for child). **Spending an NFT1 group or child token with the wrong Token Type in a SEND transaction will result in the token being burned**.

Wallet user interfaces are recommended to present NFT1 child tokens per Group ID, potentially allowing expansion to view individual tokens. The quantity of NFT1 group minting tokens should be clearly distinguished from the quantity of NFT children. While children can have different names, tickers, or document URIs from their group token, wallets need to determine how best to present this. When creating group tokens, GUIs should clarify that the minted quantity relates to the maximum number of NFT children that can be created, and default decimals to 0.




## Getting Started with SLP Development

A robust ecosystem of software and tools exists to support SLP token development and usage on Bitcoin Cash.

Wallets modified to support SLP tokens can incorporate token validation schemes to compute token balances and history alongside BCH balances. These wallets parse transactions with SLP OP_RETURN messages and apply consensus rules to filter invalid transactions and track valid tokens. Users can often configure their wallets to only view tokens they are interested in by specifying the token ID. When adding a new token, wallets should advise users to manually verify the token ID hash with a trusted source to avoid counterfeit tokens. Examples of SLP-compatible clients include Electron Cash SLP and Badger. The web wallet available at **wallet.psfoundation.cash** is a popular open-source option.

You can also store SLP tokens on **paper wallets** generated using tools like the web-based paper wallet generator available at **paperwallet.fullstack.cash**. Note that paper wallets holding tokens still require some BCH for transaction fees when sweeping the tokens.

For exploring and verifying token transactions, **simpleledger.info** is highlighted as a better block explorer for tokens compared to Blockchair. The **slp-token.fullstack.cash** web app also provides information about specific SLP tokens.

For developers, various libraries and command-line tools are available, including:
*   **BC HJs** and **bch-js**: JavaScript libraries for crafting custom transactions and interacting with the blockchain.
*   **minimal-slp-wallet**: A browser and Node.js library with basic wallet and token functions, embedding bch-js.
*   **slp-mdm**: A library specifically for creating SLP OP_RETURN messages.
*   **slpjs**: Another JavaScript library providing various SLP functionalities.
*   **psf-slp-wallet**: A command-line wallet for creating and managing tokens.
*   Tools for interacting with indexers like **psf-slp-indexer**.

These tools and libraries provide the foundation for building applications that interact with SLP tokens on Bitcoin Cash.
