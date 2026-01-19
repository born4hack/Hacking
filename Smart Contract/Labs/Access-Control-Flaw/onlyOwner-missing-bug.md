## ACCESS CONTROL FLAW

(Missing onlyOwner / permission check)

### VULNERABLE CONTRACT (COMPLETE CODE)

src/VulnerableWallet.sol

```sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract VulnerableWallet {
    address public owner;

    constructor() payable {
        owner = msg.sender;
    }

    receive() external payable {}

    function withdraw() public {
        payable(msg.sender).transfer(address(this).balance);
    }
}

```

### WHY THIS CONTRACT IS VULNERABLE (BULLET POINTS)

- withdraw() is declared public
- No check to verify who is calling
- Anyone can call withdraw()
- Function transfers entire contract balance
- Contract holds ETH → high impact
- Missing access control = critical severity


### ATTACK IMPACT (WHAT ATTACKER GAINS)

- Attacker does not need special permissions
- One transaction drains all ETH
- Owner loses full balance
- No way to recover funds

---

### TEST CONTRACT (EXPLOIT PROOF WITH LOGS)
📄 test/VulnerableWallet.t.sol


```sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "forge-std/Test.sol";
import "forge-std/console.sol";
import "../src/VulnerableWallet.sol";
import "../src/SafeWallet.sol";

contract WalletAccessControlTest is Test {
    VulnerableWallet vulnerable;
    SafeWallet safe;

    address owner = address(0x1);
    address attacker = address(0x2);

    function setUp() public {
        vm.deal(owner, 20 ether);
        vm.deal(attacker, 1 ether);

        vm.prank(owner);
        vulnerable = new VulnerableWallet{value: 10 ether}();

        vm.prank(owner);
        safe = new SafeWallet{value: 10 ether}();
    }

    function test_AttackerDrainsVulnerableWallet() public {
        console.log("=== BEFORE ATTACK ===");
        console.log("Contract balance:", address(vulnerable).balance);
        console.log("Attacker balance:", attacker.balance);

        vm.prank(attacker);
        vulnerable.withdraw();

        console.log("=== AFTER ATTACK ===");
        console.log("Contract balance:", address(vulnerable).balance);
        console.log("Attacker balance:", attacker.balance);

        assertEq(address(vulnerable).balance, 0);
    }

    function test_AttackerCannotDrainSafeWallet() public {
        vm.prank(attacker);
        vm.expectRevert("Not owner");
        safe.withdraw();
    }
}

```


### TEST CODE — WHAT EACH PART DOES (BULLET POINTS)

```sol
vm.deal(owner, 10 ether)
```

- Gives owner ETH for deployment

```sol
vm.deal(attacker, 1 ether)
```
- Gives attacker ETH for gas

```sol
new VulnerableWallet{value: 10 ether}()
```
- Deploys contract with ETH inside

```sol
vm.prank(attacker)
```
- Pretends attacker is calling the function

```sol
vulnerable.withdraw()
```
- Executes vulnerable logic

```sol
console.log(...)
```
- Shows ETH movement clearly

```sol
assertEq(address(vulnerable).balance, 0)
```
- Confirms exploit success

```sol
vm.expectRevert("Not owner")
```
- Ensures fix blocks attacker

---

## SAFE CONTRACT (COMPLETE FIXED CODE)
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

```sol
require(msg.sender == owner)
```

- Ensures only owner can call
- Unauthorized callers revert immediately
- ETH can only be sent to owner
- Attack vector fully blocked
- Minimal change → maximum security


<img width="888" height="996" alt="image" src="https://github.com/user-attachments/assets/4c212d74-1bb7-405b-9777-5f57cdc729e4" />


