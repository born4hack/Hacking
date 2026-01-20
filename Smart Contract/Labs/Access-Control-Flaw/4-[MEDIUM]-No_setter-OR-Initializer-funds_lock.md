## BAD CODE — VULNERABLE CONTRACT
📄 src/UninitializedOwner.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract UninitializedOwner {
    address public owner;

    // ❌ owner is NEVER initialized

    function withdraw() public {
        require(msg.sender == owner, "Not owner");
        payable(msg.sender).transfer(address(this).balance);
    }

    receive() external payable {}
}

```

### WHY THIS CODE IS VULNERABLE (BULLET POINTS)

- owner variable exists
- owner is never set
- Default value of address = address(0)
- No constructor
- No initializer
- Anyone who becomes owner can withdraw
- Contract looks “protected” but is not

### HOW THE ATTACK HAPPENS (BULLET POINTS)

- Contract is deployed
- owner == address(0)
- Attacker calls a function that sets owner (or exploits logic)
- Attacker becomes owner
- Attacker drains ETH
- Real users assume contract is safe

---

## TEST — PROOF OF EXPLOIT
📄 test/UninitializedOwner.t.sol



```sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "forge-std/Test.sol";
import "forge-std/console.sol";
import "../src/UninitializedOwner.sol";

contract UninitializedOwnerTest is Test {
    UninitializedOwner wallet;

    address attacker = address(0xBEEF);

    function setUp() public {
        vm.deal(attacker, 1 ether);
        wallet = new UninitializedOwner();
        vm.deal(address(wallet), 10 ether);
    }

    function test_AttackerBecomesOwner() public {
        console.log("Owner before:", wallet.owner());

        // ❌ owner is zero address
        assertEq(wallet.owner(), address(0));

        // Attacker exploits logic by becoming owner
        vm.prank(attacker);
        wallet.withdraw();

        console.log("Wallet balance after:", address(wallet).balance);
        console.log("Attacker balance after:", attacker.balance);

        assertEq(address(wallet).balance, 0);
    }
}
```


### WHAT THIS TEST PROVES (BULLET POINTS)

- Contract deployed without owner
- ETH is deposited
- owner == address(0)
- Access control check is meaningless
- Attacker drains ETH
- Contract fails despite “owner check”



---

##SAFE CODE — FIXED VERSION
📄 src/SafeInitializedOwner.sol

```sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SafeInitializedOwner {
    address public owner;

    constructor() payable {
        owner = msg.sender;
        require(owner != address(0), "Invalid owner");
    }

    function withdraw() public {
        require(msg.sender == owner, "Not owner");
        payable(owner).transfer(address(this).balance);
    }

    receive() external payable {}
}
```


### WHY THIS CODE IS SAFE (BULLET POINTS)

- Owner set during deployment
- Owner cannot be zero address
- No external initialization needed
- No takeover possible
- Access control is meaningful

---
## Test output see funds lock

<img width="1009" height="355" alt="image" src="https://github.com/user-attachments/assets/bed89f5d-10ff-456e-85c4-5b6c36c31864" />


## Summary

- Contract defines an `owner` variable
- `owner` is never initialized
- Default value of `address` is `address(0)`
- `require(msg.sender == owner)` always fails
- No function exists to set or change owner
- No one can call privileged functions
- Funds sent to the contract become permanently locked
- This is not an ownership takeover

---

## Core Lesson

- An owner check is useless if the owner is never initialized
- Uninitialized owner does not always mean an exploitable takeover
- Without a setter or initializer, the impact is fund lock
- Access control bugs depend on both state and available functions

---

## Real-World Context

- Common in poorly written contracts
- Frequently seen in upgradeable or proxy patterns
- Developers assume constructor logic exists when it does not
- Users can lose funds even without an attacker
- Auditors must distinguish between takeover bugs and fund-lock bugs

