## BAD CODE — VULNERABLE CONTRACT

📄 src/BrokenModifier.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract BrokenModifier {
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    // ❌ modifier logic is broken
    modifier onlyOwner() {
        _;
        require(msg.sender == owner, "Not owner");
    }

    function withdraw() public onlyOwner {
        (bool ok, ) = msg.sender.call{value: address(this).balance}("");
        require(ok);
    }

    receive() external payable {}
}
```


### WHY THIS CODE IS VULNERABLE

- Modifier exists → looks secure
- require is placed AFTER _
- Function body runs before the owner check
- Access control is meaningless

### HOW THE BUG HAPPENS

- Contract deployed
- ETH deposited
- Attacker calls withdraw()
- ETH is sent before ownership check
- require runs too late
- Funds already stolen

---

## TEST — PROOF OF TAKEOVER

📄 test/BrokenModifier.t.sol


```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "forge-std/Test.sol";
import "forge-std/console.sol";
import "../src/BrokenModifier.sol";

contract BrokenModifierTest is Test {
    BrokenModifier wallet;
    address attacker = address(0xBEEF);

    function setUp() public {
        wallet = new BrokenModifier();
        vm.deal(address(wallet), 5 ether);
        vm.deal(attacker, 1 ether);
    }

    function test_AttackerDrainsFunds() public {
        console.log("====== BEFORE ======");
        console.log("Wallet balance :", address(wallet).balance);
        console.log("Attacker balance:", attacker.balance);

        vm.prank(attacker);
        wallet.withdraw(); // ❌ succeeds

        console.log("====== AFTER ======");
        console.log("Wallet balance :", address(wallet).balance);
        console.log("Attacker balance:", attacker.balance);

        assertEq(address(wallet).balance, 0);
    }
}
```


### WHAT THIS TEST PROVES

- Modifier is applied
- Access control check runs after execution
- Any caller can withdraw
- Full fund loss
- Critical access control failure


---
## SAFE CODE — FIXED VERSION

📄 src/SafeModifier.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SafeModifier {
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Not owner");
        _;
    }

    function withdraw() public onlyOwner {
        (bool ok, ) = owner.call{value: address(this).balance}("");
        require(ok);
    }

    receive() external payable {}
}
```

### WHY THIS CODE IS SAFE

- Ownership check runs before function body
- Unauthorized calls revert immediately
- No execution without permission
- Funds protected
---
## SUMMARY

- Modifier exists but is incorrectly written
- _ placed before require
- Access control check runs too late
- Any user can call protected functions

## CORE LESSON

- A modifier can exist and still be completely useless
if its logic is placed in the wrong order

## REAL-WORLD CONTEXT

- Seen in rushed contracts
- Very common beginner mistake
- Found in CTFs and real audits
- Often missed in quick reviews




<img width="1137" height="394" alt="image" src="https://github.com/user-attachments/assets/9a50eeab-5d06-40ba-8198-b7022d6fb37d" />


- but here eth not theft or taken out  test failed becuase logic is wrong here mean implementation but evm protect here the contract after checking owner
- only this can possible if any state changes

- Attacker calls withdraw():

- _ runs → function body executes
- ETH transfer is attempted
- require(msg.sender == owner) runs after
- require fails → REVERT
- EVM rolls back EVERYTHING
