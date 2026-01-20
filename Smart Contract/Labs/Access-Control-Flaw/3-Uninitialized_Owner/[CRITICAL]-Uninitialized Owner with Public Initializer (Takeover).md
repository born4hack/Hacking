# Access Control Flaw — Uninitialized Owner with Public Initializer (Takeover)

---

## BAD CODE — VULNERABLE CONTRACT
📄 `src/VulnerableInitializer.sol`

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract VulnerableInitializer {
    address public owner;

    // ❌ Public initializer with no protection
    function initialize() public {
        owner = msg.sender;
    }

    function withdraw() public {
        require(msg.sender == owner, "Not owner");
        payable(msg.sender).transfer(address(this).balance);
    }

    receive() external payable {}
}
```
### WHY THIS CODE IS VULNERABLE

- owner is not set in a constructor
- Contract deploys with owner == address(0)
- A public initializer (setter) exists
- No restriction on who can call initialize()
- No check to ensure it runs only once
- First caller of initialize() becomes owner
- Full ownership takeover is possible

---


## TEST — PROOF OF TAKEOVER

📄 test/VulnerableInitializer.t.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "forge-std/Test.sol";
import "forge-std/console.sol";
import "../src/VulnerableInitializer.sol";

contract VulnerableInitializerTest is Test {
    VulnerableInitializer wallet;
    address attacker = address(0xBEEF);

    function setUp() public {
        vm.deal(attacker, 2 ether);
        wallet = new VulnerableInitializer();
        vm.deal(address(wallet), 500 ether);
    }

    function test_AttackerTakesOverAndDrains() public {
        console.log("Owner before:", wallet.owner());
		console.log("Contract balance before attack:", address(wallet).balance);
		console.log("Attacker before attack address:", address(attacker));
		console.log("attacker balance before attack:",attacker.balance);
		
		console.log("now attack------------------");
        // Attacker initializes contract
        vm.prank(attacker);
        wallet.initialize();

        console.log("Owner after init:", wallet.owner());

        // Attacker drains funds
        vm.prank(attacker);
        wallet.withdraw();

        console.log("Wallet balance:", address(wallet).balance);
        console.log("Attacker balance:", attacker.balance);

        assertEq(address(wallet).balance, 0);
    }
}


```

### WHAT THIS TEST PROVES

- Contract starts with no owner
- Attacker calls initialize() first
- Attacker becomes owner
- Access control is bypassed legitimately
- Attacker drains all ETH
- Complete contract takeover occurs

### SAFE CODE — FIXED VERSION

📄 src/SafeInitializer.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SafeInitializer {
    address public owner;
    bool private initialized;

    modifier initializer() {
        require(!initialized, "Already initialized");
        _;
        initialized = true;
    }

    function initialize(address _owner) public initializer {
        require(_owner != address(0), "Invalid owner");
        owner = _owner;
    }

    function withdraw() public {
        require(msg.sender == owner, "Not owner");
        payable(owner).transfer(address(this).balance);
    }

    receive() external payable {}
}

```

<img width="904" height="524" alt="image" src="https://github.com/user-attachments/assets/114310f0-8abb-4e64-8d55-f6b37f110250" />

---
## WHY THIS CODE IS SAFE

- Initializer can only run once
- Ownership cannot be overwritten
- Owner must be a valid address
- No attacker takeover possible
- Matches standard upgradeable patterns


---

## Summary

- Contract deploys without an owner
- A public initializer (setter) exists
- Initializer has no access control
- First caller of initializer becomes owner
- Ownership can be taken by an attacker
- Attacker can drain all contract funds



## Core Lesson

- Uninitialized owner is exploitable only when a setter exists
- Public initializers must be protected and single-use
- Access control depends on both initialization and authorization logic


## Real-World Context

- Common in upgradeable and proxy-based contracts
- Caused multiple real-world contract takeovers
- Attackers actively scan for public initializers
- One transaction can result in full ownership takeover
- Auditors must always review initializer functions



