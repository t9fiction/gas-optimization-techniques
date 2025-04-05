
# **Solidity Gas Optimization Techniques**

### **1. Minimize On-Chain Data**
Reduce the amount of data stored on-chain by using smaller or compressed formats. This cuts down on storage costs.

**Example:**
```solidity
// Inefficient
string public data = "some long data...";

// Efficient
bytes32 public dataHash = keccak256(abi.encodePacked("some long data..."));
```

---

### **2. Use Mappings Over Arrays**
Mappings allow for O(1) data retrieval, making them more efficient than arrays, especially for lookups.

**Example:**
```solidity
mapping(address => bool) public isWhitelisted; // Efficiently check if an address is whitelisted
```

---

### **3. Use `constant` and `immutable`**
`constant` variables are evaluated at compile time, while `immutable` variables are set during construction. Both save gas by not using storage slots.

**Example:**
```solidity
uint256 public constant MAX_SUPPLY = 1000; // Compile-time constant
address public immutable owner; // Set only once in constructor

constructor() {
    owner = msg.sender;
}
```

---

### **4. Optimize Unused Variables**
Remove any variables that are declared but never used to save on deployment costs and avoid unnecessary complexity.

**Example:**
```solidity
// Remove unused declarations
// uint256 unusedVar; // Not needed
```

---

### **5. Delete Unused Storage**
Using `delete` on storage variables can free up space and provide gas refunds.

**Example:**
```solidity
delete userBalances[msg.sender]; // Frees up space and refunds gas
```

---

### **6. Use Fixed-Size Arrays**
Fixed-size arrays have less overhead than dynamic arrays and should be used when the size is known.

**Example:**
```solidity
uint256[10] fixedArray; // More efficient than uint256[] dynamicArray;
```

---

### **7. Avoid Using Less Than 256-bit Variables Alone**
Using types smaller than 256 bits individually can waste gas due to padding. Prefer using `uint256` or larger types.

**Example:**
```solidity
uint256 value; // Avoid using uint8 unless packed
```

---

### **8. Pack Smaller Variables Together**
Storing smaller variables together can save storage slots by fitting them into a single 256-bit slot.

**Example:**
```solidity
struct Packed {
    uint128 a; // 16 bytes
    uint128 b; // 16 bytes
} // Both fit into one 256-bit slot
```

---

### **9. Use `external` Visibility**
Functions marked as `external` are cheaper to call from outside the contract than `public` functions.

**Example:**
```solidity
function getBalance() external view returns (uint256) {
    return balances[msg.sender]; // More efficient for external calls
}
```

---

### **10. Cache Storage Variables in Memory**
Read from storage once and store the value in a local variable to minimize repeated gas costs.

**Example:**
```solidity
uint256 balance = balances[msg.sender]; // Cache the value
// Use `balance` for subsequent operations
```

---

### **11. Avoid Initializing to Default Values**
Default values are automatically set by Solidity; initializing them to zero or false is unnecessary.

**Example:**
```solidity
uint256 x; // No need for x = 0; it is already zero
```

---

### **12. Enable Compiler Optimization**
Always enable optimization in your compiler settings to reduce gas usage during compilation.

**Example (via Hardhat or Foundry config):**
```json
"optimizer": {
  "enabled": true,
  "runs": 200 // Set for expected number of calls
}
```

---

### **13. Use Inline Assembly (Advanced)**
For critical performance sections, consider using inline assembly to optimize gas usage, but be cautious with complexity.

**Example:**
```solidity
assembly {
    result := add(x, y) // Directly add values with lower gas cost
}
```
