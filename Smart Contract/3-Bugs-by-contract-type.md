# Bugs by Smart Contract Type — Auditor Reference

This document maps **common vulnerabilities to each smart contract type**.
This is how real auditors and bug bounty hunters think:
they identify the contract type first, then hunt expected bugs.

---

## 1. Wallet / Vault Contract Bugs

### Assets at Risk
- ETH
- ERC20 tokens

### Common Bugs

- Missing access control  
  Anyone can withdraw funds.

- Reentrancy  
  Attacker withdraws multiple times before balance updates.

- Incorrect balance accounting  
  Users withdraw more than their balance.

- No withdrawal limit  
  Entire vault drained in one call.

- Unsafe ETH transfer  
  Using `call` without checks.

### Auditor Checklist
- Who can withdraw?
- Is balance updated before sending ETH?
- Can withdraw be called repeatedly?
- Are reentrancy guards used?

---

## 2. Token Contract Bugs (ERC20)

### Assets at Risk
- Token supply
- User balances

### Common Bugs

- Unlimited minting  
  Anyone can create new tokens.

- Missing access control on mint/burn  
  Supply manipulation.

- Broken approve / allowance logic  
  Attacker steals approved tokens.

- Transfer without balance check  
  Negative or incorrect balances.

- Decimal miscalculation  
  Incorrect token value or supply.

### Auditor Checklist
- Who can mint or burn?
- Can total supply change unexpectedly?
- Is allowance logic correct?
- Are balance checks enforced?

---

## 3. NFT Contract Bugs

### Assets at Risk
- NFT ownership
- Metadata integrity

### Common Bugs

- Duplicate token minting  
  Same token ID minted multiple times.

- Missing ownership check on transfer  
  Transfer someone else’s NFT.

- Metadata manipulation  
  NFT data changed after mint.

- Unauthorized burn  
  NFTs destroyed by attackers.

### Auditor Checklist
- Can tokenId be reused?
- Who can update metadata?
- Is owner verified on transfer?
- Can NFTs be burned safely?

---

## 4. DeFi Contract Bugs (General)

### Assets at Risk
- Liquidity pools
- User deposits

### Common Bugs

- Reentrancy attacks  
  Drain pools via recursive calls.

- Incorrect math logic  
  Pool imbalance or loss of funds.

- External call abuse  
  Unsafe interaction with other contracts.

- State desynchronization  
  Pool state becomes inconsistent.

### Auditor Checklist
- Are external calls protected?
- Is math precision handled?
- Can state be manipulated mid-transaction?

---

## 4A. DEX (Decentralized Exchange) Bugs

### Assets at Risk
- Liquidity pool tokens

### Common Bugs

- Price manipulation  
  Spot price used without averaging.

- Flash loan exploitation  
  Temporary liquidity manipulation.

- Slippage bypass  
  Trade executed at unfair price.

- Reentrancy in swaps  
  Pool drained during swap.

### Auditor Checklist
- Is price derived safely?
- Is TWAP used?
- Are swap functions protected?

---

## 4B. Lending / Borrowing Contract Bugs

### Assets at Risk
- Collateral
- Borrowed funds

### Common Bugs

- Under-collateralized borrowing  
  Borrow more than allowed.

- Oracle manipulation  
  Fake price inflates collateral value.

- Liquidation bypass  
  Users avoid liquidation.

- Interest calculation errors  
  Incorrect debt amounts.

### Auditor Checklist
- How is collateral value calculated?
- Can borrow limits be bypassed?
- Is liquidation enforced correctly?

---

## 4C. Staking Contract Bugs

### Assets at Risk
- Reward tokens
- Staked tokens

### Common Bugs

- Double reward claims  
  Claim rewards multiple times.

- Withdraw without staking  
  Free token extraction.

- Timestamp manipulation  
  Fake staking duration.

- Reward overflow  
  Infinite reward generation.

### Auditor Checklist
- Can rewards be claimed twice?
- Can users unstake without staking?
- Is reward math accurate?

---

## 4D. Yield Farming Contract Bugs

### Assets at Risk
- Liquidity rewards
- Incentive tokens

### Common Bugs

- Reward inflation  
  Excessive token emissions.

- Pool imbalance  
  Unequal reward distribution.

- Flash loan reward abuse  
  Farm rewards without real stake.

### Auditor Checklist
- Are rewards time-weighted?
- Can flash loans exploit farming?
- Is reward rate capped?

---

## 5. Governance / DAO Contract Bugs

### Assets at Risk
- Protocol control
- Treasury execution

### Common Bugs

- Double voting  
  Same tokens vote multiple times.

- No quorum enforcement  
  Single user passes proposal.

- Proposal replay  
  Execute same proposal again.

- Flash loan voting  
  Temporary voting power abuse.

### Auditor Checklist
- Can votes be reused?
- Is quorum enforced?
- Can proposals execute more than once?

---

## 6. Escrow Contract Bugs

### Assets at Risk
- Buyer funds
- Seller payments

### Common Bugs

- Funds locked forever  
  No valid release path.

- Cancel abuse  
  One party steals funds.

- Reentrancy on payout  
  Drain escrow.

### Auditor Checklist
- Is there always a release path?
- Who can cancel?
- Is payout safe?

---

## 7. Marketplace Contract Bugs

### Assets at Risk
- NFTs
- Payment tokens

### Common Bugs

- Listing reuse  
  Same NFT sold multiple times.

- Price manipulation  
  Change price mid-sale.

- Unauthorized sales  
  Sell NFTs without approval.

- Reentrancy on purchase  
  Drain marketplace funds.

### Auditor Checklist
- Is ownership verified before sale?
- Is listing invalidated after sale?
- Is payment transfer safe?

---

## 8. Crowdfunding / ICO Contract Bugs

### Assets at Risk
- Investor funds
- Token distribution

### Common Bugs

- Unlimited fundraising  
  No cap enforced.

- Token misallocation  
  Incorrect token distribution.

- Owner fund drain  
  Immediate withdrawal by owner.

- Refund bypass  
  Users cannot reclaim funds.

### Auditor Checklist
- Is funding cap enforced?
- Are refunds possible?
- Is token allocation correct?

---

## 9. Oracle Contract Bugs

### Assets at Risk
- Price feeds
- External data trust

### Common Bugs

- Single-source oracle  
  Easy manipulation.

- Stale price usage  
  Old data used.

- Unauthorized updates  
  Anyone can update data.

### Auditor Checklist
- Who can update data?
- Is data freshness checked?
- Are multiple sources used?

---

## 10. Upgradeable Contract Bugs

### Assets at Risk
- Entire protocol

### Common Bugs

- Unprotected upgrade function  
  Anyone can upgrade logic.

- Missing initializer protection  
  Ownership takeover.

- Storage collision  
  Corrupted state after upgrade.

### Auditor Checklist
- Who can upgrade?
- Is initializer called once?
- Is storage layout compatible?

---

## 11. Bridge Contract Bugs

### Assets at Risk
- Cross-chain tokens

### Common Bugs

- Double minting  
  Mint tokens without locking.

- Message replay  
  Reuse bridge messages.

- Validator compromise  
  Fake cross-chain validation.

### Auditor Checklist
- Is locking enforced?
- Are messages unique?
- Is validator trust minimized?

---

## 12. Multisig Contract Bugs

### Assets at Risk
- Treasury funds

### Common Bugs

- Signature reuse  
  Replay old approvals.

- Incorrect threshold logic  
  Execute with fewer signatures.

- Owner replacement abuse  
  Attacker adds self as owner.

### Auditor Checklist
- Is signature uniqueness enforced?
- Is threshold checked correctly?
- Can owners be changed safely?

---

## 13. Factory Contract Bugs

### Assets at Risk
- Deployed contracts
- User funds

### Common Bugs

- Predictable addresses  
  Precompute & exploit deployments.

- Unrestricted creation  
  Spam or malicious contracts.

- Incorrect initialization  
  Child contracts start insecure.

### Auditor Checklist
- Who can create contracts?
- Are children initialized securely?
- Are deployments predictable?

---

## Final Auditor Rule

Always follow this order:
1. Identify contract type
2. Identify assets
3. Apply expected bug checklist

This mindset is the foundation of smart contract auditing.
