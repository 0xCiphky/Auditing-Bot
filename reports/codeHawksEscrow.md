#  Automated Audit Report
## Info  
This bot is designed to streamline the auditing process by identifying basic issues in contracts allowing auditors to focus their attention on digging deeper into the codebase and uncovering more complex issues. 
  
Notice: This report may contain some false positives. Check all findings manually to verify them

## **Summary**
| Audit Date | Total Contracts Scanned | Total Findings | High | Medium | Low | Gas |
|------------|-------------------------|--------------|------------------|--------------------|-----------------|-------------------------|
| August 13, 2023 | 4 | 7 | 0 | 0 | 0 | 7 |
## Files analyzed
- 2023-07-escrow/src/Escrow.sol
- 2023-07-escrow/src/EscrowFactory.sol
- 2023-07-escrow/src/IEscrow.sol
- 2023-07-escrow/src/IEscrowFactory.sol

## **Findings Table**
| Issue Identifier | Issue | Instances |
| ---------------- | ----- | ----- |
| - | **Gas Optimization** | - |
| G003 | Use != 0 instead of > 0 for Unsigned Integer Comparison | 3 |
| G009 | Address 0 check can be done in assembly to save gas | 4 |
| G012 | Setting the constructor to payable | 1 |
| G014 | Assigning keccak operations to constant variables results in extra gas costs | 2 |
| G016 | Splitting require() statements that use && saves gas - (saves 8 gas per &&) | 1 |
| G023 | Using bool for storage incurs overhead | 4 |
| G023 | Use bytes.concat() instead of abi.encodePacked() | 4 |

## Gas Optimization - 7 Issues 19 Findings

<details><summary>Gas Optimization Findings</summary>

______
 [**G003] Use != 0 instead of > 0 for Unsigned Integer Comparison**

**Impact**  
When dealing with unsigned integer types, comparisons using != 0 are cheaper in terms of gas than using > 0.  
  
**Recommendation**  
For unsigned integer comparisons, consider using != 0 instead of > 0.
<details><summary>View Findings (3)</summary>

**File:** 2023-07-escrow/src/Escrow.sol  
**Line Number:** 119  
```sol
if (buyerAward > 0) {  
```
**[Link to Code](https://github.com/Cyfrin/2023-07-escrow/blob/main/src/Escrow.sol#L119)**  

**File:** 2023-07-escrow/src/Escrow.sol  
**Line Number:** 122  
```sol
if (i_arbiterFee > 0) {  
```
**[Link to Code](https://github.com/Cyfrin/2023-07-escrow/blob/main/src/Escrow.sol#L122)**  

**File:** 2023-07-escrow/src/Escrow.sol  
**Line Number:** 126  
```sol
if (tokenBalance > 0) {  
```
**[Link to Code](https://github.com/Cyfrin/2023-07-escrow/blob/main/src/Escrow.sol#L126)**  

</details>

______
 [**G009] Address 0 check can be done in assembly to save gas**

**Impact**  
This check can be done more efficiently using inline assembly.  
  
**Recommendation**  
Use inline assembly
<details><summary>View Findings (4)</summary>

**File:** 2023-07-escrow/src/Escrow.sol  
**Line Number:** 40  
```sol
if (address(tokenContract) == address(0)) revert Escrow__TokenZeroAddress();  
```
**[Link to Code](https://github.com/Cyfrin/2023-07-escrow/blob/main/src/Escrow.sol#L40)**  

**File:** 2023-07-escrow/src/Escrow.sol  
**Line Number:** 41  
```sol
if (buyer == address(0)) revert Escrow__BuyerZeroAddress();  
```
**[Link to Code](https://github.com/Cyfrin/2023-07-escrow/blob/main/src/Escrow.sol#L41)**  

**File:** 2023-07-escrow/src/Escrow.sol  
**Line Number:** 42  
```sol
if (seller == address(0)) revert Escrow__SellerZeroAddress();  
```
**[Link to Code](https://github.com/Cyfrin/2023-07-escrow/blob/main/src/Escrow.sol#L42)**  

**File:** 2023-07-escrow/src/Escrow.sol  
**Line Number:** 103  
```sol
if (i_arbiter == address(0)) revert Escrow__DisputeRequiresArbiter();  
```
**[Link to Code](https://github.com/Cyfrin/2023-07-escrow/blob/main/src/Escrow.sol#L103)**  

</details>

______
 [**G012] Setting the constructor to payable**

**Impact**  
You can cut out 10 opcodes in the creation-time EVM bytecode if you declare a constructor payable.  
  
**Recommendation**  
mark the constructor as payable to reduce gas costs
<details><summary>View Findings (1)</summary>

**File:** 2023-07-escrow/src/Escrow.sol  
**Line Number:** 32  
```sol
constructor(  
```
**[Link to Code](https://github.com/Cyfrin/2023-07-escrow/blob/main/src/Escrow.sol#L32)**  

</details>

______
 [**G014] Assigning keccak operations to constant variables results in extra gas costs**

**Impact**  
keccak assigned to a constant variable are re-computed at each use of the variable.  
  
**Recommendation**  
You can use an immutable variable to reduce gas costs
<details><summary>View Findings (2)</summary>

**File:** 2023-07-escrow/src/EscrowFactory.sol  
**Line Number:** 70  
```sol
keccak256(  
```
**[Link to Code](https://github.com/Cyfrin/2023-07-escrow/blob/main/src/EscrowFactory.sol#L70)**  

**File:** 2023-07-escrow/src/EscrowFactory.sol  
**Line Number:** 75  
```sol
keccak256(  
```
**[Link to Code](https://github.com/Cyfrin/2023-07-escrow/blob/main/src/EscrowFactory.sol#L75)**  

</details>

______
 [**G016] Splitting require() statements that use && saves gas - (saves 8 gas per &&)**

**Impact**  
By using multiple require() statements with one condition per require() statement, you can save 8 gas per &&. The gas difference is only realized if the revert condition is met.  
  
**Recommendation**  
You can split the require() statements to save gas
<details><summary>View Findings (1)</summary>

**File:** 2023-07-escrow/src/Escrow.sol  
**Line Number:** 67  
```sol
if (msg.sender != i_buyer && msg.sender != i_seller) {  
```
**[Link to Code](https://github.com/Cyfrin/2023-07-escrow/blob/main/src/Escrow.sol#L67)**  

</details>

______
 [**G023] Using bool for storage incurs overhead**

**Impact**  
Using the `bool` data type for storage can result in extra gas costs, especially when altering the boolean's state multiple times. Specifically, changing a `bool` from `false` to `true` after it was previously set to `true` can incur a `Gsset` charge of 20000 gas. 
  
**Recommendation**  
Consider using `uint256` values such as `uint256(1)` for `true` and `uint256(0)` for `false` to avoid these gas overheads. See [source](#) for more information.
<details><summary>View Findings (4)</summary>

**File:** 2023-07-escrow/src/EscrowFactory.sol  
**Line Number:** 71  
```sol
abi.encodePacked(  
```
**[Link to Code](https://github.com/Cyfrin/2023-07-escrow/blob/main/src/EscrowFactory.sol#L71)**  

**File:** 2023-07-escrow/src/EscrowFactory.sol  
**Line Number:** 76  
```sol
abi.encodePacked(  
```
**[Link to Code](https://github.com/Cyfrin/2023-07-escrow/blob/main/src/EscrowFactory.sol#L76)**  

**File:** 2023-07-escrow/src/EscrowFactory.sol  
**Line Number:** 71  
```sol
abi.encodePacked(  
```
**[Link to Code](https://github.com/Cyfrin/2023-07-escrow/blob/main/src/EscrowFactory.sol#L71)**  

**File:** 2023-07-escrow/src/EscrowFactory.sol  
**Line Number:** 76  
```sol
abi.encodePacked(  
```
**[Link to Code](https://github.com/Cyfrin/2023-07-escrow/blob/main/src/EscrowFactory.sol#L76)**  

</details>

______
 [**G023] Use bytes.concat() instead of abi.encodePacked()**

**Impact**  
Starting from Solidity version 0.8.4, it is recommended to use bytes.concat() for appending bytes instead of abi.encodePacked() for better readability and simplicity.  
**Recommendation** 
Replace abi.encodePacked() with bytes.concat()
<details><summary>View Findings (4)</summary>

**File:** 2023-07-escrow/src/EscrowFactory.sol  
**Line Number:** 71  
```sol
abi.encodePacked(  
```
**[Link to Code](https://github.com/Cyfrin/2023-07-escrow/blob/main/src/EscrowFactory.sol#L71)**  

**File:** 2023-07-escrow/src/EscrowFactory.sol  
**Line Number:** 76  
```sol
abi.encodePacked(  
```
**[Link to Code](https://github.com/Cyfrin/2023-07-escrow/blob/main/src/EscrowFactory.sol#L76)**  

**File:** 2023-07-escrow/src/EscrowFactory.sol  
**Line Number:** 71  
```sol
abi.encodePacked(  
```
**[Link to Code](https://github.com/Cyfrin/2023-07-escrow/blob/main/src/EscrowFactory.sol#L71)**  

**File:** 2023-07-escrow/src/EscrowFactory.sol  
**Line Number:** 76  
```sol
abi.encodePacked(  
```
**[Link to Code](https://github.com/Cyfrin/2023-07-escrow/blob/main/src/EscrowFactory.sol#L76)**  

</details>

</details>
