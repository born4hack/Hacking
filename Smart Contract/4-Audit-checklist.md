# Smart Contract Audit Checklist — Beginner to Pro

This checklist provides a **step-by-step auditing framework**
used by smart contract auditors and bug bounty hunters.
Follow this order every time you audit a contract.

---

## PHASE 0 — Pre-Audit Preparation

### Understand the Context
- What problem does the contract solve?
- Is it production code or test code?
- Is it standalone or part of a protocol?

### Collect Information
- Solidity version
- Compiler settings
- External dependencies
- Libraries used (OpenZeppelin, custom libs)

---

## PHASE 1 — Identify Contract Type

Ask:
- Is this a wallet, token, NFT, DeFi, staking, DAO, etc.?

Why this matters:
- Each contract type has **expected bugs**
- Auditing without identifying type is ineffective

Checklist:
- Identify primary contract type
- Identify secondary roles (e.g., staking + rewards)
- Identify external contracts interacted with

---

## PHASE 2 — Identify Assets at Risk

Ask:
- What valuable assets does this contract manage?

Assets may include:
- ETH
- ERC20 tokens
- NFTs
- Rewards
- Governance power
- Upgrade rights

Checklist:
- List all assets stored or controlled
- Identify who owns each asset
- Identify how assets move in and out

---

## PHASE 3 — Access Control Review (CRITICAL)

Ask:
- Who can call this function?

Checklist:
- Identify owner / admin roles
- Check `onlyOwner` usage
- Verify role-based access (if any)
- Check for missing access checks
- Verify ownership transfer logic
- Confirm no public admin functions

Red Flags:
- `public` functions that move funds
- Missing `require(msg.sender == owner)`
- Unprotected `setOwner`, `mint`, `upgrade`

---

## PHASE 4 — Function-Level Analysis

For **EVERY external/public function**, ask:

1. Who can call this?
2. Can it be called multiple times?
3. Can it be called with zero values?
4. Can it be called in the wrong order?
5. Does it transfer ETH or tokens?

Checklist:
- Trace function execution flow
- Track state changes
- Identify external calls
- Check return values
- Validate input parameters

---

## PHASE 5 — State Change & External Calls

Golden Rule:
> State changes must occur BEFORE external calls.

Checklist:
- Identify all external calls (`call`, `transfer`, `send`)
- Check order of:
  - state update
  - external interaction
- Look for reentrancy risk
- Check for reentrancy guards
- Validate callback safety

---

## PHASE 6 — Reentrancy Analysis

Ask:
- Can this function be re-entered?

Checklist:
- External call before state update?
- Multiple withdrawals possible?
- ERC777 hooks involved?
- Cross-contract reentrancy possible?

Mitigations:
- Checks-Effects-Interactions pattern
- ReentrancyGuard
- Pull over push payments

---

## PHASE 7 — Arithmetic & Logic Review

Checklist:
- Check reward calculations
- Verify division precision
- Look for rounding errors
- Validate time-based logic
- Check loop boundaries
- Identify overflow/underflow (older Solidity)

Red Flags:
- Division before multiplication
- Timestamp-based rewards
- Unbounded loops

---

## PHASE 8 — Token & NFT Specific Checks

### ERC20 Checklist
- Total supply integrity
- Mint/burn access control
- Allowance logic correctness
- Transfer restrictions
- Fee logic correctness

### NFT Checklist
- Unique tokenId enforcement
- Ownership checks on transfer
- Metadata immutability
- Safe mint/burn logic

---

## PHASE 9 — DeFi & Economic Attacks

Ask:
- Can economic behavior be abused?

Checklist:
- Price oracle source
- Flash loan resistance
- Slippage checks
- TWAP usage
- Liquidity manipulation risks
- Reward exploitation via short-term capital

---

## PHASE 10 — Oracle & External Dependency Review

Checklist:
- Oracle update authority
- Data freshness checks
- Single-source dependency
- External contract trust assumptions
- Fallback behavior if oracle fails

Red Flags:
- `setPrice()` callable by anyone
- No timestamp validation
- Single oracle source

---

## PHASE 11 — Upgradeability Review (If Applicable)

Checklist:
- Proxy pattern used?
- Upgrade function access control
- Initializer protection
- Storage layout compatibility
- Upgrade pause mechanisms

Red Flags:
- Public initialize()
- Anyone-can-upgrade logic
- Storage collision risks

---

## PHASE 12 — Failure & Edge Case Testing

Checklist:
- Zero value inputs
- Maximum value inputs
- Repeated calls
- Paused state behavior
- Emergency withdrawal logic
- Fund recovery paths

---

## PHASE 13 — Attack Simulation (Mental)

Ask:
- If I were an attacker, how would I profit?

Checklist:
- Can I steal funds?
- Can I mint value?
- Can I block withdrawals?
- Can I gain control?
- Can I grief the system?

---

## PHASE 14 — Severity Classification

Classify findings:
- Critical → Fund loss / protocol takeover
- High → Severe asset risk
- Medium → Exploitable under conditions
- Low → Minor issue
- Informational → Best practices

---

## Final Auditor Mindset

Always audit in this order:
1. Contract type
2. Asset
3. Access
4. Logic
5. External interaction
6. Economic abuse

> Smart contract auditing is not about syntax,
> it is about **thinking like an attacker**.

---
