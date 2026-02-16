
## Storage Location

In Solidity, when you create a variable (especially arrays, structs, mappings), you must specify where the data is stored.

That place is called Data Location.

There are 3 types:

- Storage
- Memory
- Calldata


## 1-Storage

Storage is the permanent data area of a smart contract.

It is:

- Saved on the blockchain
- Persistent between transactions
- Expensive (costs high gas)
- Think of it as: The contract’s hard drive


### Where Storage Is Used

- State variables (declared outside functions)
- Mappings
- Permanent balances
- Ownership data


### Example 1: Simple Variable

```solidity
contract Test {
    uint public number = 10; // Stored in STORAGE
}
```
number will stay forever unless changed.

### Example 2: Mapping

```solidity
mapping(address => uint) public balances;
```

When you do:

```solidity
balances[msg.sender] = 100;
```

This value is permanently stored on blockchain.


## Memory

Memory is a temporary data area that exists only while a function is executing.

It:

- Is deleted after function ends
- Is cheaper than storage
- Does NOT persist
- Think of it as: RAM of the contract


### Where Memory Is Used

- Temporary variables inside functions
- Struct copies
- Return values
- Calculations

### Example 1: Temporary Variable


```solidity
function test() public pure returns (uint) {
    uint x = 50; // Stored in MEMORY
    return x;
}
```

After function finishes → x disappears.


### Example 2: Struct in Memory

```solidity
struct User {
    uint balance;
}

function getUser() public pure returns (User memory) {
    User memory u = User(100);
    return u;
}
```

u exists only during function execution.


## Calldata

Calldata is a special read-only data location used for external function parameters.

It:

- Is temporary
- Cannot be modified
- Is cheaper than memory
- Used mainly in external functions
- Think of it as: Function input box

### Example

```solidity
function setNumbers(uint[] calldata arr) external {
    // arr is read-only
}
```

You cannot do:

```solidity
arr[0] = 10; // ❌ Error
```

Because calldata is read-only.


### When To Use Each One

1-Use STORAGE when:

- You want to modify permanent contract state
- Updating balances
- Saving ownership

2- Use MEMORY when:

- Temporary calculations
- Creating struct copies
- Returning values

3-Use CALLDATA when:

- Receiving external input
- Large arrays
- You don’t need to modify input

| Feature                      | Storage         | Memory          | Calldata        |
| ---------------------------- | --------------- | --------------- | --------------- |
| Permanent?                   | ✅ Yes           | ❌ No            | ❌ No            |
| Stored on Blockchain?        | ✅ Yes           | ❌ No            | ❌ No            |
| Lifetime                     | Forever         | During function | During function |
| Can Modify?                  | ✅ Yes           | ✅ Yes           | ❌ No            |
| Gas Cost                     | High            | Medium          | Low             |
| Used For                     | State variables | Temporary data  | External inputs |
| Exists Between Transactions? | ✅ Yes           | ❌ No            | ❌ No            |


### Final Simple Understanding

- Storage = Permanent (Blockchain state)
- Memory = Temporary (Function execution only)
- Calldata = Input (Read-only parameters)
