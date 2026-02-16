16-feb-2026

## Solidity Deep Knowledge (Core Foundation)

### Data Types in Solidity

### Main Types

### 1-Unsigned Integer

```solidity
uint256 public amount = 100;
```

uint public balance;
balance = 500;

Used for:

- Token balances
- Supply
- Amounts


### 2-Signed Integer

```solidity
int256 public temperature = -10;
```

Can store negative + positive numbers


### 3-Boolean

```solidity
bool public isActive = true;
```

Used for:

- Access control
- Flags
- Conditions


### 4-Address

```solidity
address public owner;

```

Stores Ethereum address.

Example: owner = msg.sender;

Used for:

- Contract ownership
- User tracking


### 5-Bytes

```solidity
bytes32 public hash;

```
Used for:

- Hash storage
- Signatures
