## BAD CODE — VULNERABLE CONTRACT

📄 src/PublicWithdraw.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract PublicWithdraw {
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    // ❌ CRITICAL FUNCTION IS PUBLIC WITH NO CHECK
    function withdraw() public {
        (bool ok, ) = msg.sender.call{value: address(this).balance}("");
        require(ok);
    }

    receive() external payable {}
}
```

### WHY THIS CODE IS VULNERABLE

- Contract has an owner
- But withdraw() has NO access control
- Function is public
- Anyone can call it
- Owner variable is useless

### HOW THE ATTACK HAPPENS

- Contract deployed
- ETH deposited
- Attacker calls withdraw()
- Contract sends all ETH to attacker
- Owner loses funds
---
## TEST — PROOF OF EXPLOIT (THIS WILL PASS)

📄 test/PublicWithdraw.t.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "forge-std/Test.sol";
import "forge-std/console.sol";
import "../src/PublicWithdraw.sol";

contract PublicWithdrawTest is Test {
    PublicWithdraw wallet;
    address attacker = address(0xBEEF);

    function setUp() public {
        wallet = new PublicWithdraw();
        vm.deal(address(wallet), 5 ether);
        vm.deal(attacker, 1 ether);
    }

    function test_AttackerDrainsFunds() public {
        console.log("====== BEFORE ======");
        console.log("Wallet balance :", address(wallet).balance);
        console.log("Attacker balance:", attacker.balance);

        vm.prank(attacker);
        wallet.withdraw(); // ✅ succeeds

        console.log("====== AFTER ======");
        console.log("Wallet balance :", address(wallet).balance);
        console.log("Attacker balance:", attacker.balance);

        assertEq(address(wallet).balance, 0);
        assertEq(attacker.balance, 6 ether);
    }
}
```

### WHAT THIS TEST PROVES

- Withdraw is callable by anyone
- No owner check exists
- Attacker drains all ETH
- Test passes successfully

---

## SAFE CODE — FIXED VERSION

📄 src/SafeWithdraw.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SafeWithdraw {
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
<img width="1192" height="888" alt="image" src="https://github.com/user-attachments/assets/d4d85f17-a442-4213-a15e-84bcb00e4ed6" />



## SUMMARY

- Critical function was public
- No access control applied
- Owner variable existed but unused
- Anyone could drain funds

## CORE LESSON

- If a critical function is public without checks, access control does not exist.

## REAL-WORLD CONTEXT
- Extremely common beginner mistake
- Seen in real hacks
- Often missed in quick reviews
- One of the first bugs auditors look for

## SEVERITY

- CRITICAL (Direct fund theft)


