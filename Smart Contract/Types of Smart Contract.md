# Smart Contract Types — Detailed Explanation with Examples

This document explains all major smart contract types,
their purpose, real-world usage, and how to think about them
as a beginner and future smart contract auditor.

---

## 1. Wallet / Vault Contract

### Purpose
- Store ETH or tokens securely
- Allow withdrawals based on rules

### Explanation
A wallet or vault contract holds funds on behalf of users or a protocol.
Instead of a private key controlling funds, **logic controls access**.

Funds are released only when conditions are met
(e.g., owner-only, multisig approval, time-lock).

### Real-World Examples
- Team treasury wallet
- Protocol reserve vault
- DAO fund storage

### One-Line Summary
> A contract that safely stores crypto and controls withdrawals.

---

## 2. Token Contract

### Purpose
- Create and manage tokens
- Control transfers, minting, and burning

### Explanation
Token contracts define **what a token is** and **how it behaves**.
They keep track of balances and total supply.

### Token Standards
- ERC20 → Fungible tokens (currencies)
- ERC721 → NFTs (unique items)
- ERC1155 → Multiple token types in one contract

### Real-World Examples
- USDT (ERC20)
- Governance tokens
- Game currencies

### One-Line Summary
> A contract that defines how tokens exist and move.

---

## 3. NFT Contract

### Purpose
- Represent unique digital ownership

### Explanation
NFT contracts assign **unique token IDs** to owners.
Each NFT represents something unique (art, land, item).

Ownership is tracked on-chain.

### Real-World Examples
- Digital art NFTs
- Game items
- Event tickets

### One-Line Summary
> A contract where each token is unique and owned by one address.

---

## 4. DeFi Contract (Decentralized Finance)

### Meaning
- Financial services without banks

### Explanation
DeFi contracts replace traditional finance logic
using code instead of intermediaries.

Users interact directly with contracts to trade,
lend, borrow, or earn rewards.

### One-Line Summary
> Smart contracts that provide financial services on blockchain.

---

### 4A. DEX (Decentralized Exchange)

### Purpose
- Swap tokens without a centralized exchange

### Explanation
DEX contracts allow users to trade tokens
using liquidity pools instead of order books.

Prices are determined by formulas.

### Real-World Examples
- Token swap platforms
- Liquidity pools

### One-Line Summary
> A contract that lets users trade tokens directly.

---

### 4B. Lending / Borrowing Contract

### Purpose
- Lend assets to earn interest
- Borrow assets using collateral

### Explanation
Users deposit tokens as collateral
and can borrow other assets against them.

If collateral value drops, liquidation occurs.

### Real-World Examples
- Crypto lending platforms
- Stablecoin borrowing

### One-Line Summary
> A decentralized bank that manages loans and collateral.

---

### 4C. Staking Contract

### Purpose
- Lock tokens to earn rewards

### Explanation
Users stake tokens in a contract.
Over time, they earn rewards based on amount and duration.

### Real-World Examples
- Proof-of-stake rewards
- Protocol incentive systems

### One-Line Summary
> Lock tokens now to earn rewards later.

---

### 4D. Yield Farming Contract

### Purpose
- Earn rewards by providing liquidity

### Explanation
Users provide liquidity to protocols
and earn reward tokens as incentives.

More complex than staking.

### Real-World Examples
- Liquidity mining programs
- Incentive-based DeFi platforms

### One-Line Summary
> Earn rewards by putting tokens to work.

---

## 5. Governance / DAO Contract

### Purpose
- Voting and decision-making

### Explanation
Governance contracts allow token holders
to vote on proposals that affect a protocol.

Votes are counted and executed on-chain.

### Real-World Examples
- DAO voting systems
- Protocol upgrades

### One-Line Summary
> Token holders vote and control the protocol.

---

## 6. Escrow Contract

### Purpose
- Hold funds until conditions are met

### Explanation
Escrow contracts lock funds
until both sides meet agreed conditions.

They remove the need for trust.

### Real-World Examples
- Freelance payments
- NFT purchases

### One-Line Summary
> Funds are locked until rules are satisfied.

---

## 7. Marketplace Contract

### Purpose
- Buy and sell NFTs or tokens

### Explanation
Marketplace contracts manage listings,
payments, and ownership transfers.

They act as trusted middlemen.

### Real-World Examples
- NFT marketplaces
- Digital asset stores

### One-Line Summary
> A middleman contract for trading assets.

---

## 8. Crowdfunding / ICO Contract

### Purpose
- Raise funds from users

### Explanation
Users send ETH or tokens to the contract
and receive project tokens in return.

Often time-limited.

### Real-World Examples
- Token launches
- Fundraising campaigns

### One-Line Summary
> Users fund a project and receive tokens.

---

## 9. Oracle Contract

### Purpose
- Bring off-chain data on-chain

### Explanation
Blockchains cannot access the internet.
Oracle contracts feed external data into smart contracts.

### Real-World Examples
- Token price feeds
- Weather-based contracts
- Sports betting

### One-Line Summary
> Blockchain’s connection to real-world data.

---

## 10. Upgradeable Contract

### Purpose
- Change logic after deployment

### Explanation
Logic and data are separated.
The contract can be upgraded without changing address.

### Real-World Examples
- Long-lived protocols
- Continuously improving platforms

### One-Line Summary

> Same address, new logic.

---

## 11. Bridge Contract

### Purpose
- Move assets between blockchains

### Explanation
Tokens are locked on one chain
and minted or unlocked on another.

### Real-World Examples
- Ethereum ↔ BSC bridges
- Cross-chain transfers

### One-Line Summary
> Lock tokens on one chain, mint on another.

---

## 12. Multisig Contract

### Purpose
- Require multiple approvals

### Explanation
Actions require approval from multiple owners.
Prevents single point of failure.

### Real-World Examples
- DAO treasuries
- Company wallets

### One-Line Summary
> Multiple signatures required to execute actions.

---

## 13. Factory Contract

### Purpose
- Deploy multiple contracts

### Explanation
Factory contracts create new contracts dynamically.
Used for scalability and standardization.

### Real-World Examples
- NFT mint factories
- Pair creation contracts

### One-Line Summary
> A contract that creates other contracts.

---

## Final Auditor Tip

Always ask:
1. What type of contract is this?
2. What asset does it protect?
3. Who controls it?

Understanding types is the foundation of smart contract security.
