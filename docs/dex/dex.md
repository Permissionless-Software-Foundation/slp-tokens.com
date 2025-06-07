---
sidebar_position: 1
---

# DEX

DEX stands for Decentralized EXchange. SLP tokens have a DEX, but it does not operate in the same way as a DEX on EVM chains like Ethereum. The SLP DEX does not use liquidity pools. Instead there are only two parties involved (the buyer and seller). Trades happen atomically and trustlessly.

The SLP DEX is based on the [SWaP protocol](https://github.com/vinarmani/swap-protocol/blob/master/swap-protocol-spec.md).


## bch-dex

The SWaP protocol is implemented using node software called [bch-dex](https://github.com/Permissionless-Software-Foundation/bch-dex). There are two front end clients, one for Buyers and one for Sellers.

You can read more on the [DEX documentation site](https://dex-docs.psfoundation.info/).

- [DEX Buyer App](https://dex.psfoundation.info)
- [DEX Seller App](https://dex-seller.psfoundation.info)

## SWaP Protocol

The SWaP protocol, or Signal, Watch, and Pay Protocol, is designed for on-chain communication between parties. Its purpose is to allow peer-to-peer, permissionless, trustless negotiation and execution of collaborative transactions. This is achieved by using a blockchain-based messaging scheme.

The SWaP protocol defines three types of collaborative transactions:

- SLP Atomic Swaps: Exchanges of Bitcoin Cash (BCH) satoshis for units of a given Simple Ledger Protocol token. This is the core transaction type utilized by the SLP DEX.
- Multi-Party Escrow: Pay-To-Script-Hash outputs where spending relies on data broadcast on-chain by an oracle.
- Threshold Crowdfunding: Trustless multi-input transactions that are only valid if the aggregate value of all submitted inputs meets a predefined output value.

The protocol facilitates these collaborative transactions through a sequence of actions involving Signal, Watch, and Pay messages:

1. Signal: An Offering Party broadcasts a Signal message to the network to indicate a desire to collaborate on a transaction. For an SLP Atomic Swap, this Signal contains structured metadata within an OP_RETURN output, including the SLP token ID, whether the offer is to buy or sell tokens for BCH, the exchange rate in satoshis, information about the exact UTXO being offered, and the minimum amount to exchange.

2. Watch: The Accepting Party watches the network for a desired Signal or searches the blockchain for one matching a certain pattern.

3. Pay: Using the information from the Signal, the Accepting Party constructs a collaborative transaction, signs their portion of the inputs, and broadcasts a Payment message. This Payment message contains the raw data of the partially-signed transaction, encapsulated within OP_RETURN messages.

4. Watch (again): The Offering Party watches for broadcast Payment messages that correspond to their original Signal.

5. Final Broadcast: Upon finding a matching Payment, the Offering Party downloads the partially signed transaction data and validates it to ensure it fulfills the requirements of their original Signal. If valid, they sign their own inputs and broadcast the complete, valid transaction to the blockchain. If the transaction is accepted by the network, the exchange occurs atomically, and the Offering Party may spend a Baton UTXO from their original Signal to mark the offer as spent.

This process allows the buyer and seller in the SLP DEX to agree upon and execute a trade directly on the blockchain without needing a central exchange or liquidity pool.

Software applications like bch-dex-taker-v2 are designed to help users browse and purchase NFTs (a type of SLP token) for sale on the DEX network. These applications implement the SWaP protocol logic for the "Accepting Party" role, including watching for Signals and broadcasting Payment messages. Other software, such as bch-dex (a node) and bch-dex-ui-v2 (a web app), allows users to sell SLP tokens and NFTs on the DEX network. These tools facilitate the "Offering Party" role, enabling users to create and broadcast Signal messages, watch for incoming Payments, validate them, and broadcast the final signed transaction.

The DEX leverages the existing infrastructure for handling SLP tokens, including wallets like bch-wallet-web3-spa and minimal-slp-wallet for managing BCH and SLP assets, and indexers such as psf-slp-indexer for tracking SLP token data on the blockchain. SLP tokens use OP_RETURN messages for transaction metadata and are associated with BCH outputs, typically minimal 'dust' amounts. While accidental token burning is possible if not handled correctly, modern wallet software is designed to prevent this.