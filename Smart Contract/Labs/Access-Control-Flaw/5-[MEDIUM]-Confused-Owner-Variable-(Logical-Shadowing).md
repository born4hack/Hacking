## BAD CODE — VULNERABLE CONTRACT

📄 src/ConfusedOwner.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract ConfusedOwner {
    address public owner;
    address public admin; // ❌ wrong variable used

    constructor() {
        owner = msg.sender; // real owner
    }

    function withdraw() public {
        // ❌ access control checks wrong variable
        require(msg.sender == admin, "Not admin");

        (bool ok, ) = msg.sender.call{value: address(this).balance}("");
        require(ok);
    }

    receive() external payable {}
}
```

### WHY THIS CODE IS VULNERABLE

- owner is correctly initialized
- admin is a different variable
- admin is never set
- Withdraw checks admin instead of owner
- Real owner cannot withdraw
- Funds become locked

### HOW THE BUG HAPPENS

- Contract deployed
- owner = deployer
- admin = address(0)
- Withdraw checks admin

No caller satisfies condition

ETH stuck forever

---

## TEST — PROOF OF FUND LOCK

📄 test/ConfusedOwner.t.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "forge-std/Test.sol";
import "forge-std/console.sol";
import "../src/ConfusedOwner.sol";

contract ConfusedOwnerTest is Test {
    ConfusedOwner wallet;

    address ownerAddr;
    address adminAddr;

    function setUp() public {
        wallet = new ConfusedOwner();
        vm.deal(address(wallet), 5 ether);

        ownerAddr = wallet.owner();   // real owner
        adminAddr = wallet.admin();   // zero address
    }

    function test_FundsLocked() public {
        console.log("====== BEFORE WITHDRAW ======");
        console.log("Owner address        :", ownerAddr);
        console.log("Owner balance        :", ownerAddr.balance);
        console.log("Admin address        :", adminAddr);
        console.log("Admin balance        :", adminAddr.balance);
        console.log("Wallet balance       :", address(wallet).balance);

        // sanity checks
        assertEq(adminAddr, address(0));
        assertEq(address(wallet).balance, 5 ether);

        // withdraw must fail
        vm.expectRevert();
        wallet.withdraw();

        console.log("====== AFTER WITHDRAW ======");
        console.log("Owner address        :", ownerAddr);
        console.log("Owner balance        :", ownerAddr.balance);
        console.log("Admin address        :", adminAddr);
        console.log("Admin balance        :", adminAddr.balance);
        console.log("Wallet balance       :", address(wallet).balance);

        // funds still locked
        assertEq(address(wallet).balance, 5 ether);
    }
}
```

### WHAT THIS TEST PROVES

- Owner exists but is ignored
- Wrong variable used in access control
- No one can withdraw
- Contract fails logically
- Funds permanently locked

---

## SAFE CODE — FIXED VERSION

📄 src/SafeOwner.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SafeOwner {
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    function withdraw() public {
        require(msg.sender == owner, "Not owner");

        (bool ok, ) = owner.call{value: address(this).balance}("");
        require(ok);
    }

    receive() external payable {}
}
```

### WHY THIS CODE IS SAFE

- Single authority variable
- Owner initialized correctly
- Correct variable checked
- No access confusion
- Funds accessible only to owner
---
## SUMMARY

- Contract defines a valid owner
- Withdraw checks the wrong variable
- Access control becomes meaningless
- Funds get locked
- Bug is logical, not syntactic

## CORE LESSON

- Checking the wrong variable in access control
is the same as having no access control

## REAL-WORLD CONTEXT

- Seen in rushed refactors
- Happens when migrating legacy contracts
- Common in custom RBAC systems
- Often missed during manual reviews
- Auditors flag as “confused authority”

## SEVERITY

- MEDIUM (Funds locked, no direct takeover)

## poc 

<img width="1157" height="368" alt="image" src="https://github.com/user-attachments/assets/f1c9ed56-a289-4c28-84ec-2965c254c1c0" />

