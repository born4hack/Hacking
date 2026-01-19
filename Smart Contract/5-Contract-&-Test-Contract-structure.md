

## BASIC SMART CONTRACT STRUCTURE

---
- File: src/SimpleCounter.sol  (our deployed contract)

```sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SimpleCounter {
    uint256 public count;

    constructor() {
        count = 0;
    }

    function increment() public {
        count = count + 1;
    }
}

```

What this smart contract is

Think of this as the real program you want to test.

Line-by-line (simple)


```sol
contract SimpleCounter {

```

Start of the contract (like a class)


```sol
uint256 public count;
```

A number stored on blockchain
public means we can read it using count()

```sol
constructor() {
    count = 0;
}
```

Runs once when contract is deployed
Initializes count

```sol
function increment() public {
    count = count + 1;
}

```

- Anyone can call this
- It increases the stored number


What this contract does

- Stores a number
- Starts at 0
- Increases by 1 when increment() is called

---


## BASIC TEST CONTRACT STRUCTURE

File: test/SimpleCounter.t.sol


```sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "forge-std/Test.sol";
import "forge-std/console.sol";
import "../src/SimpleCounter.sol";

contract SimpleCounterTest is Test {
    SimpleCounter counter;

    function setUp() public {
        counter = new SimpleCounter();
    }

    function test_Increment() public {
        console.log("Initial count:", counter.count());

        counter.increment();

        console.log("After increment:", counter.count());

        assertEq(counter.count(), 1);
    }
}

```

What this test contract is

Think of this as a controller that:

- deploys the contract
- calls its functions
- checks results


### Line by line explanation

```sol
import "../src/SimpleCounter.sol";
```

- Makes the test aware of the contract we are testing

```sol
contract SimpleCounterTest is Test {
```

- This is a test contract
- is Test gives testing tools

```sol
SimpleCounter counter;
```


- A variable that will store
- the address of the deployed SimpleCounter contract

```sol
function setUp() public {
    counter = new SimpleCounter();
}
```

- Deploys SimpleCounter
- Saves its address into counter
- This is where the connection is created

```sol
function test_Increment() public {
```


- A test function
- Foundry automatically runs it

```sol
counter.increment();
```

- Calls increment() on the real deployed contract

```sol

assertEq(counter.count(), 1);

```

- Reads value from the contract
- Confirms state changed correctly


- git bash interface and command you can verify

<img width="636" height="958" alt="image" src="https://github.com/user-attachments/assets/c946f542-057b-4c73-a8bc-ce630865f867" />
