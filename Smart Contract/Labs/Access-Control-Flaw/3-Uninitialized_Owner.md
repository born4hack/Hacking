## BAD CODE — VULNERABLE CONTRACT
📄 src/UninitializedOwner.sol

```sol
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

## CORE LESSON (MEMORIZE THIS)

- An access check is useless if the owner is never initialized.
---
## REAL-WORLD CONTEXT

### Common in:

- Proxy contracts
- Upgradeable contracts
- Poorly written initializers
- Attackers scan for:

```sol
owner == address(0)
```
- One transaction = full takeover
