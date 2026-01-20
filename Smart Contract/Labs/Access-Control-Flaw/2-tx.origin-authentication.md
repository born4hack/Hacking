## BAD CODE — VULNERABLE WALLET (tx.origin BUG)
📄 src/TxOriginWallet.sol


```sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract TxOriginWallet {
    address public owner;

    constructor() payable {
        owner = msg.sender;
    }

    receive() external payable {}

    function withdraw() public {
        require(tx.origin == owner, "Not owner");
        payable(msg.sender).transfer(address(this).balance);
    }
}


```
### WHY THIS CODE IS BAD (BULLET POINTS)

- Contract holds ETH
- Owner is stored correctly
- Uses tx.origin for authentication ❌
- tx.origin = wallet that started transaction
- Does NOT check who is calling right now
- Sends ETH to msg.sender
- Allows phishing / malicious contract attacks

---

## ATTACKER CONTRACT (REQUIRED TO EXPLOIT)
📄 src/TxOriginAttacker.sol

```sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "./TxOriginWallet.sol";

contract TxOriginAttacker {
    TxOriginWallet public wallet;

    constructor(TxOriginWallet _wallet) {
        wallet = _wallet;
    }

    function attack() public {
        wallet.withdraw();
    }

    receive() external payable {}
}

```

### WHAT THIS CONTRACT DOES (BULLET POINTS)

- Simulates malicious / phishing contract
- Stores address of vulnerable wallet
- Has attack() function
- Calls wallet.withdraw()
- Becomes msg.sender inside wallet
- Receives stolen ETH


### WHY ATTACKER CONTRACT IS NEEDED (BULLET POINTS)

- tx.origin bug only works with contract-to-contract calls
- Direct attacker call would fail
- Owner must be tricked into calling attacker
- Attacker contract sits between owner and wallet
- This matches real-world phishing attacks

---

## TEST FILE (THIS IS WHAT YOU ASKED FOR)
📄 test/TxOriginWallet.t.sol

```sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "forge-std/Test.sol";
import "forge-std/console.sol";

import "../src/TxOriginWallet.sol";
import "../src/TxOriginAttacker.sol";
import "../src/SafeWallet.sol";

contract TxOriginAccessControlTest is Test {
    TxOriginWallet wallet;
    TxOriginAttacker attackerContract;
    SafeWallet safe;

    address owner = address(0x1);
    address attacker = address(0x2);

    function setUp() public {
        vm.deal(owner, 20 ether);
        vm.deal(attacker, 1 ether);

        vm.prank(owner);
        wallet = new TxOriginWallet{value: 10 ether}();

        vm.prank(attacker);
        attackerContract = new TxOriginAttacker(wallet);

        vm.prank(owner);
        safe = new SafeWallet{value: 10 ether}();
    }

    function test_TxOriginAttackSucceeds() public {
        console.log("=== BEFORE ATTACK ===");
        console.log("Wallet balance:", address(wallet).balance);
        console.log("Attacker contract balance:", address(attackerContract).balance);

        vm.startPrank(owner, owner);
        attackerContract.attack();
        vm.stopPrank();

        console.log("=== AFTER ATTACK ===");
        console.log("Wallet balance:", address(wallet).balance);
        console.log("Attacker contract balance:", address(attackerContract).balance);

        assertEq(address(wallet).balance, 0);
    }

    function test_AttackFailsOnSafeWallet() public {
        vm.prank(attacker);
        vm.expectRevert("Not owner");
        safe.withdraw();
    }
}

```

### WHAT THIS TEST PROVES (BULLETS)

- Owner deploys wallet with ETH
- Attacker deploys malicious contract
- Owner is tricked into calling attacker
- tx.origin == owner → check passes
- msg.sender == attacker contract
- Wallet sends ETH to attacker
- Same attack FAILS on SafeWallet
---

## SAFE CODE — CORRECT ACCESS CONTROL
📄 src/SafeWallet.sol

```sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SafeWallet {
    address public owner;

    constructor() payable {
        owner = msg.sender;
    }

    receive() external payable {}

    function withdraw() public {
        require(msg.sender == owner, "Not owner");
        payable(owner).transfer(address(this).balance);
    }
}


```

### WHY THIS CODE IS SAFE (BULLET POINTS)

- Uses msg.sender for authentication
- Checks immediate caller
- Attacker contract ≠ owner
- Phishing attack fails
- ETH always goes to owner
- Industry-standard pattern
---

## CORE DIFFERENCE (ONE GLANCE)


| Bad Wallet            | Safe Wallet            |
| --------------------- | ---------------------- |
| `tx.origin`           | `msg.sender`           |
| Phishable             | Not phishable          |
| Sends to caller       | Sends to owner         |
| Broken access control | Correct access control |


## FINAL ONE-LINE SUMMARY (MEMORIZE THIS)

- tx.origin checks who STARTED the transaction, but msg.sender checks who is CALLING now — access control must always use msg.sender.

<img width="824" height="355" alt="image" src="https://github.com/user-attachments/assets/68da7f25-1539-4f59-a3a4-b1f3a615773e" />
