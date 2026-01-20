# ACCESS CONTROL BUGS — COMPLETE LIST (TILL DATE)

This document lists **all known Access Control vulnerability categories** encountered in
smart contract audits, CTFs, bug bounties (Immunefi, Code4rena), and real-world hacks.

---

## BASIC / COMMON

- Missing OnlyOwner  
- Public Critical Function  
- Incorrect Visibility (public / external)  
- Missing Modifier  
- Broken Modifier Logic  
- Modifier Not Applied  
- Wrong Function Selector Protected  

---

## AUTHENTICATION LOGIC ERRORS

- tx.origin Authentication  
- msg.sender Misuse  
- Hardcoded Owner  
- Zero Address Owner  
- Uninitialized Owner  
- Owner Can Be Overwritten  
- Constructor Not Setting Owner  

---

## VARIABLE / STORAGE ISSUES

- Shadowed Owner Variable  
- Duplicate Owner Variable  
- Storage Collision (Proxy)  
- Incorrect Storage Slot  
- Delegatecall Storage Takeover  

---

## ROLE-BASED ACCESS CONTROL (RBAC)

- Missing Role Check  
- Wrong Role Checked  
- Role Granted to Anyone  
- Role Revocation Missing  
- Admin Role Not Protected  
- Role Escalation  
- Default Admin Misuse  
- OpenZeppelin RBAC Misconfiguration  

---

## MULTI-CONTRACT / CROSS-CONTRACT

- Trusted Contract Assumption  
- Unverified External Caller  
- Call Chain Privilege Escalation  
- Cross-Contract Owner Confusion  
- Delegatecall to Untrusted Contract  

---

## UPGRADEABILITY / PROXY RELATED

- Unprotected Upgrade Function  
- Proxy Admin Takeover  
- Implementation Initialization Bug  
- Reinitialization Attack  
- Missing onlyProxy / onlyImplementation  
- UUPS Authorization Bug  

---

## GOVERNANCE / TIME / STATE

- Timelock Bypass  
- Governance Vote Bypass  
- Snapshot Manipulation  
- State-Dependent Access Control  
- Paused State Bypass  
- Emergency Function Unrestricted  

---

## LOGICAL / BUSINESS ACCESS

- Business Logic Access Bypass  
- Condition-Based Access Bug  
- If/Else Access Fallthrough  
- Boolean Flag Misuse  
- Access via Fallback / Receive  
- Access via Selfdestruct  

---

## EDGE / ADVANCED (REAL HACKS)

- Signature Replay for Access  
- EIP-712 Domain Confusion  
- Meta-Tx Access Confusion  
- Permit-Based Privilege Escalation  
- Oracle-Controlled Access  
- Flashloan-Assisted Access Bypass  

---

## Notes

- All vulnerabilities listed above fall strictly under **Access Control**.
- Most real-world exploits involve **misconfigured permissions**, not complex math bugs.
- This list is suitable for:
  - Smart contract auditing
  - Bug bounty hunting
  - Learning & practice roadmap
  - GitHub documentation

---

