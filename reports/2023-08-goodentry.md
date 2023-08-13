#  Automated Audit Report: Good Entry (Code4rena)
## Info  
This bot is designed to streamline the auditing process by identifying basic issues in contracts allowing auditors to focus their attention on digging deeper into the codebase and uncovering more complex issues. 
  
Notice: This report may contain some false positives. Check all findings manually to verify them

## **Summary**
| Audit Date | Total Contracts Scanned | Total Findings | High | Medium | Low | Gas |
|------------|-------------------------|--------------|------------------|--------------------|-----------------|-------------------------|
| August 13, 2023 | 10 | 22 | 0 | 3 | 4 | 15 |
## Files analyzed
- 2023-08-goodentry/contracts/GeVault.sol
- 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol
- 2023-08-goodentry/contracts/PositionManager/PositionManager.sol
- 2023-08-goodentry/contracts/RangeManager.sol
- 2023-08-goodentry/contracts/RoeRouter.sol
- 2023-08-goodentry/contracts/TokenisableRange.sol
- 2023-08-goodentry/contracts/helper/FixedOracle.sol
- 2023-08-goodentry/contracts/helper/LPOracle.sol
- 2023-08-goodentry/contracts/helper/OracleConvert.sol
- 2023-08-goodentry/contracts/helper/V3Proxy.sol

## **Findings Table**
| Issue Identifier | Issue | Instances |
| ---------------- | ----- | ----- |
| - | **Medium Risk** | - |
| M001 | Unsafe ERC20 Operation(s) | 3 |
| M002 | Potential Issue with .transfer() or .send() for Ether Transfer | 1 |
| M003 | Use safeMint in place of mint | 1 |
| - | **Low Risk** | - |
| L002 | Unspecific Compiler Version Pragma | 2 |
| L003 | Do not use Deprecated Library Functions | 8 |
| L005 | Use safeTransferOwnership instead of transferOwnership function | 8 |
| L007 | Use of bytes.concat() instead of abi.encodePacked() | 4 |
| - | **Gas Optimization** | - |
| G001 | Don't Initialize Variables with Default Value | 9 |
| G002 | Cache Array Length Outside of Loop | 7 |
| G003 | Use != 0 instead of > 0 for Unsigned Integer Comparison | 36 |
| G005 | Long Revert Strings | 1 |
| G006 | Use Shift Right/Left instead of Division/Multiplication if possible | 17 |
| G007 | ++i costs less gas than i++, especially when its used in for-loops (--i/i-- too) | 8 |
| G008 | Use custom errors rather than revert()/require() strings to save gas | 79 |
| G009 | Address 0 check can be done in assembly to save gas | 18 |
| G011 | Functions guaranteed to revert when called by normal users can be marked payable | 12 |
| G012 | Setting the constructor to payable | 9 |
| G015 | x += y costs more gas than x = x + y for state variables | 21 |
| G016 | Splitting require() statements that use && saves gas - (saves 8 gas per &&) | 26 |
| G017 | ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow | 8 |
| G022 | Using private for constants saves gas | 1 |
| G023 | Using bool for storage incurs overhead | 2 |

## Medium Risk - 3 Issues 5 Findings

<details><summary>Medium Risk Findings</summary>

______
 [**M001] Unsafe ERC20 Operation(s)**

**Impact**  
Unsafe ERC20 operations, such as transfer(), transferFrom(), and approve(), may return false on failure, which could lead to unexpected behavior if not handled correctly.  
**Recommendation**  
Use safer alternatives such as OpenZeppelin's SafeERC20 library to mitigate this risk.
<details><summary>View Findings (3)</summary>

**File:** 2023-08-goodentry/contracts/RangeManager.sol  
**Line Number:** 165  
```sol
tr.approve(address(LENDING_POOL), lpAmt);  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RangeManager.sol#L165)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 228  
```sol
TOKEN0.token.transferFrom(msg.sender, address(this), n0);  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L228)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 229  
```sol
TOKEN1.token.transferFrom(msg.sender, address(this), n1);  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L229)**  

</details>

______
 [**M002] Potential Issue with .transfer() or .send() for Ether Transfer**

**Impact**  
Using .transfer() or .send() has a gas stipend limitation of 2300, which might not be sufficient for the recipient contract, potentially causing unexpected failures. 
**Recommendation**  
Consider using .call{value: _amount}("") for Ether transfers, ensuring to implement necessary safety checks to handle potential errors.
<details><summary>View Findings (1)</summary>

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 232  
```sol
payable(msg.sender).transfer(bal);  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L232)**  

</details>

______
 [**M003] Use safeMint in place of mint**

**Impact**  
The safeMint function includes additional checks and validations compared to the basic mint function, ensuring that tokens are minted only to valid, non-zero addresses.  
**Recommendation**  
SafeMint should be used in place of mint.
<details><summary>View Findings (1)</summary>

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 142  
```sol
(tokenId, liquidity, , ) = POS_MGR.mint(  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L142)**  

</details>

</details>

## Low Risk - 4 Issues 22 Findings

<details><summary>Low Risk Findings</summary>

______
 [**L002] Unspecific Compiler Version Pragma**

**Impact**  
Unspecific compiler version pragma can lead to potential security risks and unexpected behavior when new compiler versions are released.  
**Recommendation**  
It is recommended to use a fixed version pragma for non-library contracts to ensure consistency and maintainability.
<details><summary>View Findings (2)</summary>

**File:** 2023-08-goodentry/contracts/RangeManager.sol  
**Line Number:** 2  
```sol
pragma solidity ^0.8.0;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RangeManager.sol#L2)**  

**File:** 2023-08-goodentry/contracts/helper/FixedOracle.sol  
**Line Number:** 1  
```sol
pragma solidity ^0.8.0;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/FixedOracle.sol#L1)**  

</details>

______
 [**L003] Do not use Deprecated Library Functions**

**Impact**  
The use of safeApprove has been deprecated in favour of safeIncreaseAllowance and safeDecreaseAllowance.  
**Recommendation**  
It is recommended to replace deprecated functions with their updated counterparts to ensure the contract remains secure and up-to-date.
<details><summary>View Findings (8)</summary>

**File:** 2023-08-goodentry/contracts/helper/V3Proxy.sol  
**Line Number:** 116  
```sol
ogInAsset.safeApprove(address(ROUTER), amountIn);  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/V3Proxy.sol#L116)**  

**File:** 2023-08-goodentry/contracts/helper/V3Proxy.sol  
**Line Number:** 120  
```sol
ogInAsset.safeApprove(address(ROUTER), 0);  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/V3Proxy.sol#L120)**  

**File:** 2023-08-goodentry/contracts/helper/V3Proxy.sol  
**Line Number:** 128  
```sol
ogInAsset.safeApprove(address(ROUTER), amountInMax);  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/V3Proxy.sol#L128)**  

**File:** 2023-08-goodentry/contracts/helper/V3Proxy.sol  
**Line Number:** 133  
```sol
ogInAsset.safeApprove(address(ROUTER), 0);  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/V3Proxy.sol#L133)**  

**File:** 2023-08-goodentry/contracts/helper/V3Proxy.sol  
**Line Number:** 165  
```sol
ogInAsset.safeApprove(address(ROUTER), amountInMax);  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/V3Proxy.sol#L165)**  

**File:** 2023-08-goodentry/contracts/helper/V3Proxy.sol  
**Line Number:** 169  
```sol
ogInAsset.safeApprove(address(ROUTER), 0);  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/V3Proxy.sol#L169)**  

**File:** 2023-08-goodentry/contracts/helper/V3Proxy.sol  
**Line Number:** 183  
```sol
ogInAsset.safeApprove(address(ROUTER), amountIn);  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/V3Proxy.sol#L183)**  

**File:** 2023-08-goodentry/contracts/helper/V3Proxy.sol  
**Line Number:** 187  
```sol
ogInAsset.safeApprove(address(ROUTER), 0);  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/V3Proxy.sol#L187)**  

</details>

______
 [**L005] Use safeTransferOwnership instead of transferOwnership function**

**Impact**  
**Recommendation**
<details><summary>View Findings (8)</summary>

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 4  
```sol
import "./openzeppelin-solidity/contracts/access/Ownable.sol";  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L4)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 27  
```sol
contract GeVault is ERC20, Ownable, ReentrancyGuard {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L27)**  

**File:** 2023-08-goodentry/contracts/RangeManager.sol  
**Line Number:** 20  
```sol
import "./openzeppelin-solidity/contracts/access/Ownable.sol";  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RangeManager.sol#L20)**  

**File:** 2023-08-goodentry/contracts/RangeManager.sol  
**Line Number:** 25  
```sol
contract RangeManager is ReentrancyGuard, Ownable {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RangeManager.sol#L25)**  

**File:** 2023-08-goodentry/contracts/RoeRouter.sol  
**Line Number:** 4  
```sol
import "./openzeppelin-solidity/contracts/access/Ownable.sol";  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RoeRouter.sol#L4)**  

**File:** 2023-08-goodentry/contracts/RoeRouter.sol  
**Line Number:** 10  
```sol
contract RoeRouter is Ownable {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RoeRouter.sol#L10)**  

**File:** 2023-08-goodentry/contracts/helper/V3Proxy.sol  
**Line Number:** 4  
```sol
import "../openzeppelin-solidity/contracts/access/Ownable.sol";  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/V3Proxy.sol#L4)**  

**File:** 2023-08-goodentry/contracts/helper/V3Proxy.sol  
**Line Number:** 60  
```sol
contract V3Proxy is ReentrancyGuard, Ownable {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/V3Proxy.sol#L60)**  

</details>

______
 [**L007] Use of bytes.concat() instead of abi.encodePacked()**

**Impact**  
Starting from Solidity version 0.8.4, it is recommended to use bytes.concat() for appending bytes instead of abi.encodePacked() for better readability and simplicity.  
**Recommendation** 
Replace abi.encodePacked() with bytes.concat()
<details><summary>View Findings (4)</summary>

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 108  
```sol
_name     = string(abi.encodePacked("Ticker ", baseSymbol, " ", quoteSymbol, " ", startName, "-", endName));  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L108)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 109  
```sol
_symbol    = string(abi.encodePacked("T-",startName,"_",endName,"-",baseSymbol,"-",quoteSymbol));  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L109)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 114  
```sol
_name     = string(abi.encodePacked("Ranger ", baseSymbol, " ", quoteSymbol, " ", startName, "-", endName));  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L114)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 115  
```sol
_symbol   = string(abi.encodePacked("R-",startName,"_",endName,"-",baseSymbol,"-",quoteSymbol));  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L115)**  

</details>

</details>

## Gas Optimization - 15 Issues 254 Findings

<details><summary>Gas Optimization Findings</summary>

______
 [**G001] Don't Initialize Variables with Default Value**

**Impact**  
Initializing variables with their default values is redundant and can lead to unnecessary gas consumption.  
  
**Recommendation**  
Avoid initializing state variables with default values.
<details><summary>View Findings (9)</summary>

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 154  
```sol
for (uint k = 0; k < ticks.length - 2; k++)  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L154)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 299  
```sol
for (uint k = 0; k < ticks.length; k++){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L299)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 314  
```sol
for (uint k = 0; k < ticks.length; k++){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L314)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 393  
```sol
for(uint k=0; k<ticks.length; k++){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L393)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 71  
```sol
for ( uint8 k = 0; k<assets.length; k++){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L71)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 93  
```sol
for ( uint8 k =0; k<assets.length; k++){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L93)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 172  
```sol
for (uint8 i = 0; i< options.length; ){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L172)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 203  
```sol
for (uint8 i = 0; i< options.length; ){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L203)**  

**File:** 2023-08-goodentry/contracts/RangeManager.sol  
**Line Number:** 62  
```sol
for (uint i = 0; i < len; i++) {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RangeManager.sol#L62)**  

</details>

______
 [**G002] Cache Array Length Outside of Loop**

**Impact**  
Caching the length of an array outside the loop can optimize gas usage, as the function only needs to retrieve the value once.  
  
**Recommendation**  
Cache the length of the array outside the loop to optimize gas usage.
<details><summary>View Findings (7)</summary>

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 299  
```sol
for (uint k = 0; k < ticks.length; k++){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L299)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 314  
```sol
for (uint k = 0; k < ticks.length; k++){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L314)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 393  
```sol
for(uint k=0; k<ticks.length; k++){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L393)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 71  
```sol
for ( uint8 k = 0; k<assets.length; k++){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L71)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 93  
```sol
for ( uint8 k =0; k<assets.length; k++){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L93)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 172  
```sol
for (uint8 i = 0; i< options.length; ){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L172)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 203  
```sol
for (uint8 i = 0; i< options.length; ){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L203)**  

</details>

______
 [**G003] Use != 0 instead of > 0 for Unsigned Integer Comparison**

**Impact**  
When dealing with unsigned integer types, comparisons using != 0 are cheaper in terms of gas than using > 0.  
  
**Recommendation**  
For unsigned integer comparisons, consider using != 0 instead of > 0.
<details><summary>View Findings (36)</summary>

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 218  
```sol
require(liquidity > 0, "GEV: Withdraw Zero");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L218)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 252  
```sol
require(amount > 0 || msg.value > 0, "GEV: Deposit Zero");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L252)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 255  
```sol
if (msg.value > 0){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L255)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 281  
```sol
require(liquidity > 0, "GEV: No Liquidity Added");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L281)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 329  
```sol
if (aBal > 0){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L329)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 346  
```sol
if (amount1ft > 0){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L346)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 352  
```sol
if (availToken0 > 0){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L352)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 356  
```sol
if (availToken1 > 0){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L356)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 410  
```sol
if (bal > 0){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L410)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 434  
```sol
if ( (amt0 == 0 && amt0n > 0) || (amt1 == 0 && amt1n > 0) )  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L434)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 234  
```sol
if ( repayAmount > 0 && repayAmount < debt ) debt = repayAmount;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L234)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 235  
```sol
require(debt > 0, "OPM: No Debt");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L235)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 473  
```sol
if (amount > 0 && AmountsRouter(address(ammRouter)).getAmountsOut(amount, path)[1] > 0){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L473)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 495  
```sol
require( amountsIn[0] <= maxAmount && amountsIn[0] > 0, "OPM: Invalid Swap Amounts" );  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L495)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 523  
```sol
require ( priceAssetA > 0 && priceAssetB > 0, "OPM: Invalid Oracle Price");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L523)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 525  
```sol
require( amountB > 0, "OPM: Target Amount Too Low");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L525)**  

**File:** 2023-08-goodentry/contracts/PositionManager/PositionManager.sol  
**Line Number:** 91  
```sol
if (amt > 0) {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/PositionManager.sol#L91)**  

**File:** 2023-08-goodentry/contracts/PositionManager/PositionManager.sol  
**Line Number:** 96  
```sol
if ( debt > 0 ){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/PositionManager.sol#L96)**  

**File:** 2023-08-goodentry/contracts/PositionManager/PositionManager.sol  
**Line Number:** 125  
```sol
if ( amount > 0 ){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/PositionManager.sol#L125)**  

**File:** 2023-08-goodentry/contracts/RangeManager.sol  
**Line Number:** 112  
```sol
if (trAmt > 0) {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RangeManager.sol#L112)**  

**File:** 2023-08-goodentry/contracts/RangeManager.sol  
**Line Number:** 124  
```sol
if (trAmt > 0) {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RangeManager.sol#L124)**  

**File:** 2023-08-goodentry/contracts/RangeManager.sol  
**Line Number:** 153  
```sol
if (amount0 > 0) {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RangeManager.sol#L153)**  

**File:** 2023-08-goodentry/contracts/RangeManager.sol  
**Line Number:** 158  
```sol
if (amount1 > 0) {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RangeManager.sol#L158)**  

**File:** 2023-08-goodentry/contracts/RangeManager.sol  
**Line Number:** 194  
```sol
if (asset0_amt > 0) {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RangeManager.sol#L194)**  

**File:** 2023-08-goodentry/contracts/RangeManager.sol  
**Line Number:** 199  
```sol
if (asset1_amt > 0) {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RangeManager.sol#L199)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 180  
```sol
if (tf0 > 0) TOKEN0.token.safeTransfer(treasury, tf0);  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L180)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 181  
```sol
if (tf1 > 0) TOKEN1.token.safeTransfer(treasury, tf1);  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L181)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 225  
```sol
require(totalSupply() > 0, "TR Closed");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L225)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 237  
```sol
if ( fee0+fee1 > 0 && ( n0 > 0 || fee0 == 0) && ( n1 > 0 || fee1 == 0 ) ){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L237)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 241  
```sol
if (token0Amount + fee0 > 0) newFee0 = n0 * fee0 / (token0Amount + fee0);  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L241)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 242  
```sol
if (token1Amount + fee1 > 0) newFee1 = n1 * fee1 / (token1Amount + fee1);  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L242)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 271  
```sol
require (TOKEN0_PRICE > 0 && TOKEN1_PRICE > 0, "Invalid Oracle Price");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L271)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 315  
```sol
if (fee0 > 0) {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L315)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 320  
```sol
if (fee1 > 0) {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L320)**  

**File:** 2023-08-goodentry/contracts/helper/LPOracle.sol  
**Line Number:** 63  
```sol
require(timeStamp > 0, "Round not complete");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/LPOracle.sol#L63)**  

**File:** 2023-08-goodentry/contracts/helper/OracleConvert.sol  
**Line Number:** 44  
```sol
require(timeStamp > 0, "Round not complete");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/OracleConvert.sol#L44)**  

</details>

______
 [**G005] Long Revert Strings**

**Impact**  
For the output on a require, each extra memory word of bytes past the original 32 incurs an MSTORE operation which costs 3 gas.  
  
**Recommendation**  
Use a shorter revert string
<details><summary>View Findings (1)</summary>

**File:** 2023-08-goodentry/contracts/helper/FixedOracle.sol  
**Line Number:** 11  
```sol
require(msg.sender == owner, "Only the owner can call this function.");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/FixedOracle.sol#L11)**  

</details>

______
 [**G006] Use Shift Right/Left instead of Division/Multiplication if possible**

**Impact**  
Using shift operations instead of division or multiplication can save gas when dealing with powers of two.  
  
**Recommendation**  
Consider using shift operations for these cases.
<details><summary>View Findings (17)</summary>

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 353  
```sol
depositAndStash(ticks[tick0Index], availToken0 / 2, 0);  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L353)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 354  
```sol
depositAndStash(ticks[tick0Index+1], availToken0 / 2, 0);  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L354)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 357  
```sol
depositAndStash(ticks[tick1Index], 0, availToken1 / 2);  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L357)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 358  
```sol
depositAndStash(ticks[tick1Index+1], 0, availToken1 / 2);  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L358)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 374  
```sol
priceX8 = priceX8 * uint(sqrtPriceX96 / 2 ** 12) ** 2 * 1e8 / 2**168;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L374)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 456  
```sol
if (adjustedBaseFeeX4 < baseFeeX4 / 2) adjustedBaseFeeX4 = baseFeeX4 / 2;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L456)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 457  
```sol
if (adjustedBaseFeeX4 > baseFeeX4 * 3 / 2) adjustedBaseFeeX4 = baseFeeX4 * 3 / 2;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L457)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 67  
```sol
uint z = (x + 1) / 2;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L67)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 71  
```sol
z = (x / z + z) / 2;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L71)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 99  
```sol
int24 _upperTick = TickMath.getTickAtSqrtRatio( uint160( 2**48 * sqrt( (2 ** 96 * (10 ** TOKEN1.decimals)) * 1e10 / (uint256(startX10) * 10 ** TOKEN0.decimals) ) ) );  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L99)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 100  
```sol
int24 _lowerTick = TickMath.getTickAtSqrtRatio( uint160( 2**48 * sqrt( (2 ** 96 * (10 ** TOKEN1.decimals)) * 1e10 / (uint256(endX10  ) * 10 ** TOKEN0.decimals) ) ) );  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L100)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 105  
```sol
midleTick = (_upperTick + _lowerTick) / 2;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L105)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 106  
```sol
_upperTick = (midleTick + int24(feeTier)) - (midleTick + int24(feeTier)) % int24(feeTier * 2);  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L106)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 112  
```sol
_lowerTick = (_lowerTick + int24(feeTier)) - (_lowerTick + int24(feeTier)) % int24(feeTier * 2);  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L112)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 113  
```sol
_upperTick = (_upperTick + int24(feeTier)) - (_upperTick + int24(feeTier)) % int24(feeTier * 2);  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L113)**  

**File:** 2023-08-goodentry/contracts/helper/LPOracle.sol  
**Line Number:** 45  
```sol
uint z = (x + 1) / 2;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/LPOracle.sol#L45)**  

**File:** 2023-08-goodentry/contracts/helper/LPOracle.sol  
**Line Number:** 49  
```sol
z = (x / z + z) / 2;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/LPOracle.sol#L49)**  

</details>

______
 [**G007] ++i costs less gas than i++, especially when its used in for-loops (--i/i-- too)**

**Impact**  
Using ++i or --i (pre-increment/decrement) is generally more gas-efficient than using i++ or i--.  
  
**Recommendation**  
Use pre-increment/decrement
<details><summary>View Findings (8)</summary>

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 154  
```sol
for (uint k = 0; k < ticks.length - 2; k++)  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L154)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 299  
```sol
for (uint k = 0; k < ticks.length; k++){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L299)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 314  
```sol
for (uint k = 0; k < ticks.length; k++){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L314)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 393  
```sol
for(uint k=0; k<ticks.length; k++){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L393)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 431  
```sol
for (activeTickIndex = 0; activeTickIndex < ticks.length - 3; activeTickIndex++){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L431)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 71  
```sol
for ( uint8 k = 0; k<assets.length; k++){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L71)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 93  
```sol
for ( uint8 k =0; k<assets.length; k++){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L93)**  

**File:** 2023-08-goodentry/contracts/RangeManager.sol  
**Line Number:** 62  
```sol
for (uint i = 0; i < len; i++) {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RangeManager.sol#L62)**  

</details>

______
 [**G008] Use custom errors rather than revert()/require() strings to save gas**

**Impact**  
From Solidity version 0.8.4 onwards, custom errors can be used instead of revert() and require() strings to save gas. Custom errors save approximately 50 gas each time they are hit, by avoiding the need to allocate and store the revert string.  
  
**Recommendation**  
Use custom errors
<details><summary>View Findings (79)</summary>

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 79  
```sol
require(_treasury != address(0x0), "GEV: Invalid Treasury");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L79)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 80  
```sol
require(_uniswapPool != address(0x0), "GEV: Invalid Pool");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L80)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 81  
```sol
require(weth != address(0x0), "GEV: Invalid WETH");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L81)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 120  
```sol
require(t0 == token0 && t1 == token1, "GEV: Invalid TR");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L120)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 125  
```sol
require( t.lowerTick() > ticks[ticks.length-1].upperTick(), "GEV: Push Tick Overlap");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L125)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 127  
```sol
require( t.upperTick() < ticks[ticks.length-1].lowerTick(), "GEV: Push Tick Overlap");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L127)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 141  
```sol
require(t0 == token0 && t1 == token1, "GEV: Invalid TR");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L141)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 146  
```sol
require( t.lowerTick() > ticks[0].upperTick(), "GEV: Shift Tick Overlap");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L146)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 148  
```sol
require( t.upperTick() < ticks[0].lowerTick(), "GEV: Shift Tick Overlap");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L148)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 170  
```sol
require(t0 == token0 && t1 == token1, "GEV: Invalid TR");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L170)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 184  
```sol
require(newBaseFeeX4 < 1e4, "GEV: Invalid Base Fee");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L184)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 203  
```sol
require(poolMatchesOracle(), "GEV: Oracle Error");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L203)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 215  
```sol
require(poolMatchesOracle(), "GEV: Oracle Error");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L215)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 217  
```sol
require(liquidity <= balanceOf(msg.sender), "GEV: Insufficient Balance");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L217)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 218  
```sol
require(liquidity > 0, "GEV: Withdraw Zero");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L218)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 249  
```sol
require(isEnabled, "GEV: Pool Disabled");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L249)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 250  
```sol
require(poolMatchesOracle(), "GEV: Oracle Error");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L250)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 251  
```sol
require(token == address(token0) || token == address(token1), "GEV: Invalid Token");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L251)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 252  
```sol
require(amount > 0 || msg.value > 0, "GEV: Deposit Zero");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L252)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 256  
```sol
require(token == address(WETH), "GEV: Invalid Weth");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L256)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 269  
```sol
require(tvlCap > valueX8 + getTVL(), "GEV: Max Cap Reached");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L269)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 281  
```sol
require(liquidity > 0, "GEV: No Liquidity Added");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L281)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 69  
```sol
require( address(lendingPool) == msg.sender, "OPM: Call Unallowed");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L69)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 91  
```sol
require( address(lendingPool) == msg.sender, "OPM: Call Unallowed");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L91)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 137  
```sol
require(sourceSwap == token0 || sourceSwap == token1, "OPM: Invalid Swap Token");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L137)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 167  
```sol
require(options.length == amounts.length && sourceSwap.length == options.length, "OPM: Array Length Mismatch");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L167)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 198  
```sol
require(options.length == amounts.length, "ARRAY_LEN_MISMATCH");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L198)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 235  
```sol
require(debt > 0, "OPM: No Debt");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L235)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 261  
```sol
require(collateralAsset == token0 || collateralAsset == token1, "OPM: Invalid Collateral Asset");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L261)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 344  
```sol
require(  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L344)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 369  
```sol
require(feeAmount <= IERC20(collateralAsset).balanceOf(address(this)), "OPM: Insufficient Collateral");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L369)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 393  
```sol
require( LP.getReserveData(optionAddress).aTokenAddress != address(0x0), "OPM: Invalid Address" );  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L393)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 420  
```sol
require( LP.getReserveData(optionAddress).aTokenAddress != address(0x0), "OPM: Invalid Address" );  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L420)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 441  
```sol
require(sourceAsset == token0 || sourceAsset == token1, "OPM: Invalid Swap Asset");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L441)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 495  
```sol
require( amountsIn[0] <= maxAmount && amountsIn[0] > 0, "OPM: Invalid Swap Amounts" );  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L495)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 496  
```sol
require( amountsIn[0] <= ERC20(path[0]).balanceOf(address(this)), "OPM: Insufficient Token Amount" );  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L496)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 523  
```sol
require ( priceAssetA > 0 && priceAssetB > 0, "OPM: Invalid Oracle Price");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L523)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 525  
```sol
require( amountB > 0, "OPM: Target Amount Too Low");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L525)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 536  
```sol
require(token0 == address(t0) && token1 == address(t1), "OPM: Invalid Debt Asset");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L536)**  

**File:** 2023-08-goodentry/contracts/PositionManager/PositionManager.sol  
**Line Number:** 30  
```sol
require(roerouter != address(0x0), "Invalid address");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/PositionManager.sol#L30)**  

**File:** 2023-08-goodentry/contracts/PositionManager/PositionManager.sol  
**Line Number:** 145  
```sol
require( valueA <= valueB * 101 / 100, "PM: LP Oracle Error");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/PositionManager.sol#L145)**  

**File:** 2023-08-goodentry/contracts/PositionManager/PositionManager.sol  
**Line Number:** 146  
```sol
require( valueB <= valueA * 101 / 100, "PM: LP Oracle Error");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/PositionManager.sol#L146)**  

**File:** 2023-08-goodentry/contracts/RangeManager.sol  
**Line Number:** 49  
```sol
require( address(lendingPool) != address(0x0), "Invalid address" );  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RangeManager.sol#L49)**  

**File:** 2023-08-goodentry/contracts/RangeManager.sol  
**Line Number:** 60  
```sol
require(start < end, "Range invalid");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RangeManager.sol#L60)**  

**File:** 2023-08-goodentry/contracts/RangeManager.sol  
**Line Number:** 76  
```sol
require(beacon != address(0x0), "Invalid beacon");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RangeManager.sol#L76)**  

**File:** 2023-08-goodentry/contracts/RangeManager.sol  
**Line Number:** 108  
```sol
require(step < tokenisedRanges.length && step < tokenisedTicker.length, "Invalid step");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RangeManager.sol#L108)**  

**File:** 2023-08-goodentry/contracts/RangeManager.sol  
**Line Number:** 206  
```sol
require(hf > 1.01e18, "Health factor is too low");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RangeManager.sol#L206)**  

**File:** 2023-08-goodentry/contracts/RoeRouter.sol  
**Line Number:** 34  
```sol
require(treasury_ != address(0x0), "Invalid address");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RoeRouter.sol#L34)**  

**File:** 2023-08-goodentry/contracts/RoeRouter.sol  
**Line Number:** 68  
```sol
require (  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RoeRouter.sol#L68)**  

**File:** 2023-08-goodentry/contracts/RoeRouter.sol  
**Line Number:** 75  
```sol
require(token0 < token1, "Invalid Order");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RoeRouter.sol#L75)**  

**File:** 2023-08-goodentry/contracts/RoeRouter.sol  
**Line Number:** 84  
```sol
require(newTreasury != address(0x0), "Invalid address");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RoeRouter.sol#L84)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 86  
```sol
require(address(_oracle) != address(0x0), "Invalid oracle");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L86)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 87  
```sol
require(status == ProxyState.INIT_PROXY, "!InitProxy");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L87)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 135  
```sol
require(status == ProxyState.INIT_LP, "!InitLP");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L135)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 136  
```sol
require(msg.sender == creator, "Unallowed call");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L136)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 209  
```sol
require(addedValue > liquidityValue * 95 / 100 && liquidityValue > addedValue * 95 / 100, "TR: Claim Fee Slippage");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L209)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 225  
```sol
require(totalSupply() > 0, "TR Closed");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L225)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 271  
```sol
require (TOKEN0_PRICE > 0 && TOKEN1_PRICE > 0, "Invalid Oracle Price");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L271)**  

**File:** 2023-08-goodentry/contracts/helper/FixedOracle.sol  
**Line Number:** 11  
```sol
require(msg.sender == owner, "Only the owner can call this function.");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/FixedOracle.sol#L11)**  

**File:** 2023-08-goodentry/contracts/helper/LPOracle.sol  
**Line Number:** 29  
```sol
require(lpToken != address(0x0) && clToken0 != address(0x0) && clToken1 != address(0x0), "Invalid address");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/LPOracle.sol#L29)**  

**File:** 2023-08-goodentry/contracts/helper/LPOracle.sol  
**Line Number:** 63  
```sol
require(timeStamp > 0, "Round not complete");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/LPOracle.sol#L63)**  

**File:** 2023-08-goodentry/contracts/helper/LPOracle.sol  
**Line Number:** 101  
```sol
require(decimalsA <= 18 && decimalsB <= 18, "Incorrect tokens");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/LPOracle.sol#L101)**  

**File:** 2023-08-goodentry/contracts/helper/OracleConvert.sol  
**Line Number:** 23  
```sol
require(clToken0 != address(0x0) && clToken1 != address(0x0), "Invalid address");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/OracleConvert.sol#L23)**  

**File:** 2023-08-goodentry/contracts/helper/OracleConvert.sol  
**Line Number:** 26  
```sol
require(CL_TOKENA.decimals() + CL_TOKENB.decimals() >= 16, "Decimals error");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/OracleConvert.sol#L26)**  

**File:** 2023-08-goodentry/contracts/helper/OracleConvert.sol  
**Line Number:** 44  
```sol
require(timeStamp > 0, "Round not complete");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/OracleConvert.sol#L44)**  

**File:** 2023-08-goodentry/contracts/helper/V3Proxy.sol  
**Line Number:** 87  
```sol
require(acceptPayable, "CannotReceiveETH");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/V3Proxy.sol#L87)**  

**File:** 2023-08-goodentry/contracts/helper/V3Proxy.sol  
**Line Number:** 91  
```sol
require(acceptPayable, "CannotReceiveETH");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/V3Proxy.sol#L91)**  

**File:** 2023-08-goodentry/contracts/helper/V3Proxy.sol  
**Line Number:** 99  
```sol
require(path.length == 2, "Direct swap only");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/V3Proxy.sol#L99)**  

**File:** 2023-08-goodentry/contracts/helper/V3Proxy.sol  
**Line Number:** 106  
```sol
require(path.length == 2, "Direct swap only");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/V3Proxy.sol#L106)**  

**File:** 2023-08-goodentry/contracts/helper/V3Proxy.sol  
**Line Number:** 113  
```sol
require(path.length == 2, "Direct swap only");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/V3Proxy.sol#L113)**  

**File:** 2023-08-goodentry/contracts/helper/V3Proxy.sol  
**Line Number:** 125  
```sol
require(path.length == 2, "Direct swap only");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/V3Proxy.sol#L125)**  

**File:** 2023-08-goodentry/contracts/helper/V3Proxy.sol  
**Line Number:** 138  
```sol
require(path.length == 2, "Direct swap only");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/V3Proxy.sol#L138)**  

**File:** 2023-08-goodentry/contracts/helper/V3Proxy.sol  
**Line Number:** 139  
```sol
require(path[0] == ROUTER.WETH9(), "Invalid path");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/V3Proxy.sol#L139)**  

**File:** 2023-08-goodentry/contracts/helper/V3Proxy.sol  
**Line Number:** 148  
```sol
require(path.length == 2, "Direct swap only");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/V3Proxy.sol#L148)**  

**File:** 2023-08-goodentry/contracts/helper/V3Proxy.sol  
**Line Number:** 149  
```sol
require(path[0] == ROUTER.WETH9(), "Invalid path");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/V3Proxy.sol#L149)**  

**File:** 2023-08-goodentry/contracts/helper/V3Proxy.sol  
**Line Number:** 161  
```sol
require(path.length == 2, "Direct swap only");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/V3Proxy.sol#L161)**  

**File:** 2023-08-goodentry/contracts/helper/V3Proxy.sol  
**Line Number:** 162  
```sol
require(path[1] == ROUTER.WETH9(), "Invalid path");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/V3Proxy.sol#L162)**  

**File:** 2023-08-goodentry/contracts/helper/V3Proxy.sol  
**Line Number:** 179  
```sol
require(path.length == 2, "Direct swap only");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/V3Proxy.sol#L179)**  

**File:** 2023-08-goodentry/contracts/helper/V3Proxy.sol  
**Line Number:** 180  
```sol
require(path[1] == ROUTER.WETH9(), "Invalid path");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/V3Proxy.sol#L180)**  

</details>

______
 [**G009] Address 0 check can be done in assembly to save gas**

**Impact**  
This check can be done more efficiently using inline assembly.  
  
**Recommendation**  
Use inline assembly
<details><summary>View Findings (18)</summary>

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 79  
```sol
require(_treasury != address(0x0), "GEV: Invalid Treasury");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L79)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 80  
```sol
require(_uniswapPool != address(0x0), "GEV: Invalid Pool");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L80)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 81  
```sol
require(weth != address(0x0), "GEV: Invalid WETH");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L81)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 136  
```sol
if (sourceSwap != address(0) ){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L136)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 393  
```sol
require( LP.getReserveData(optionAddress).aTokenAddress != address(0x0), "OPM: Invalid Address" );  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L393)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 420  
```sol
require( LP.getReserveData(optionAddress).aTokenAddress != address(0x0), "OPM: Invalid Address" );  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L420)**  

**File:** 2023-08-goodentry/contracts/PositionManager/PositionManager.sol  
**Line Number:** 30  
```sol
require(roerouter != address(0x0), "Invalid address");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/PositionManager.sol#L30)**  

**File:** 2023-08-goodentry/contracts/RangeManager.sol  
**Line Number:** 49  
```sol
require( address(lendingPool) != address(0x0), "Invalid address" );  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RangeManager.sol#L49)**  

**File:** 2023-08-goodentry/contracts/RangeManager.sol  
**Line Number:** 76  
```sol
require(beacon != address(0x0), "Invalid beacon");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RangeManager.sol#L76)**  

**File:** 2023-08-goodentry/contracts/RoeRouter.sol  
**Line Number:** 34  
```sol
require(treasury_ != address(0x0), "Invalid address");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RoeRouter.sol#L34)**  

**File:** 2023-08-goodentry/contracts/RoeRouter.sol  
**Line Number:** 69  
```sol
lendingPoolAddressProvider != address(0x0)  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RoeRouter.sol#L69)**  

**File:** 2023-08-goodentry/contracts/RoeRouter.sol  
**Line Number:** 70  
```sol
&& token0 != address(0x0)  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RoeRouter.sol#L70)**  

**File:** 2023-08-goodentry/contracts/RoeRouter.sol  
**Line Number:** 71  
```sol
&& token1 != address(0x0)  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RoeRouter.sol#L71)**  

**File:** 2023-08-goodentry/contracts/RoeRouter.sol  
**Line Number:** 72  
```sol
&& ammRouter != address(0x0),  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RoeRouter.sol#L72)**  

**File:** 2023-08-goodentry/contracts/RoeRouter.sol  
**Line Number:** 84  
```sol
require(newTreasury != address(0x0), "Invalid address");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RoeRouter.sol#L84)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 86  
```sol
require(address(_oracle) != address(0x0), "Invalid oracle");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L86)**  

**File:** 2023-08-goodentry/contracts/helper/LPOracle.sol  
**Line Number:** 29  
```sol
require(lpToken != address(0x0) && clToken0 != address(0x0) && clToken1 != address(0x0), "Invalid address");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/LPOracle.sol#L29)**  

**File:** 2023-08-goodentry/contracts/helper/OracleConvert.sol  
**Line Number:** 23  
```sol
require(clToken0 != address(0x0) && clToken1 != address(0x0), "Invalid address");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/OracleConvert.sol#L23)**  

</details>

______
 [**G011] Functions guaranteed to revert when called by normal users can be marked payable**

**Impact**  
Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.  
  
**Recommendation**  
You can mark the function as payable to reduce gas costs.
<details><summary>View Findings (12)</summary>

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 101  
```sol
function setEnabled(bool _isEnabled) public onlyOwner {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L101)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 108  
```sol
function setTreasury(address newTreasury) public onlyOwner {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L108)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 116  
```sol
function pushTick(address tr) public onlyOwner {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L116)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 137  
```sol
function shiftTick(address tr) public onlyOwner {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L137)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 167  
```sol
function modifyTick(address tr, uint index) public onlyOwner {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L167)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 183  
```sol
function setBaseFee(uint newBaseFeeX4) public onlyOwner {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L183)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 191  
```sol
function setTvlCap(uint newTvlCap) public onlyOwner {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L191)**  

**File:** 2023-08-goodentry/contracts/RangeManager.sol  
**Line Number:** 75  
```sol
function generateRange(uint128 startX10, uint128 endX10, string memory startName, string memory endName, address beacon) external onlyOwner {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RangeManager.sol#L75)**  

**File:** 2023-08-goodentry/contracts/RangeManager.sol  
**Line Number:** 95  
```sol
function initRange(address tr, uint amount0, uint amount1) external onlyOwner {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RangeManager.sol#L95)**  

**File:** 2023-08-goodentry/contracts/RoeRouter.sol  
**Line Number:** 48  
```sol
function deprecatePool(uint poolId) public onlyOwner {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RoeRouter.sol#L48)**  

**File:** 2023-08-goodentry/contracts/RoeRouter.sol  
**Line Number:** 83  
```sol
function setTreasury(address newTreasury) public onlyOwner {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RoeRouter.sol#L83)**  

**File:** 2023-08-goodentry/contracts/helper/FixedOracle.sol  
**Line Number:** 24  
```sol
function setHardcodedPrice(int256 _hardcodedPrice) external onlyOwner {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/FixedOracle.sol#L24)**  

</details>

______
 [**G012] Setting the constructor to payable**

**Impact**  
You can cut out 10 opcodes in the creation-time EVM bytecode if you declare a constructor payable.  
  
**Recommendation**  
mark the constructor as payable to reduce gas costs
<details><summary>View Findings (9)</summary>

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 67  
```sol
constructor(  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L67)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 25  
```sol
constructor (address roerouter) PositionManager(roerouter) {}  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L25)**  

**File:** 2023-08-goodentry/contracts/PositionManager/PositionManager.sol  
**Line Number:** 29  
```sol
constructor(address roerouter) {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/PositionManager.sol#L29)**  

**File:** 2023-08-goodentry/contracts/RangeManager.sol  
**Line Number:** 48  
```sol
constructor(ILendingPool lendingPool, ERC20 _asset0, ERC20 _asset1)  {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RangeManager.sol#L48)**  

**File:** 2023-08-goodentry/contracts/RoeRouter.sol  
**Line Number:** 33  
```sol
constructor (address treasury_) {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RoeRouter.sol#L33)**  

**File:** 2023-08-goodentry/contracts/helper/FixedOracle.sol  
**Line Number:** 15  
```sol
constructor(int256 _hardcodedPrice) {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/FixedOracle.sol#L15)**  

**File:** 2023-08-goodentry/contracts/helper/LPOracle.sol  
**Line Number:** 28  
```sol
constructor (address lpToken, address clToken0, address clToken1 ){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/LPOracle.sol#L28)**  

**File:** 2023-08-goodentry/contracts/helper/OracleConvert.sol  
**Line Number:** 22  
```sol
constructor (address clToken0, address clToken1 ){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/OracleConvert.sol#L22)**  

**File:** 2023-08-goodentry/contracts/helper/V3Proxy.sol  
**Line Number:** 75  
```sol
constructor(ISwapRouter _router, IQuoter _quoter, uint24 _fee) {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/V3Proxy.sol#L75)**  

</details>

______
 [**G015] x += y costs more gas than x = x + y for state variables**

**Impact**  
Using x += y for state variables costs more gas than using x = x + y. This is because the += and -= operators require extra gas for updating the state variable compared to the standard assignment operator.   
  
**Recommendation**  
You can use x = x + y to reduce gas costs
<details><summary>View Findings (21)</summary>

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 304  
```sol
amount0 += amt0;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L304)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 305  
```sol
amount1 += amt1;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L305)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 396  
```sol
valueX8 += bal * t.latestAnswer() / 1e18;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L396)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 174  
```sol
unchecked { i+=1; }  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L174)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 205  
```sol
unchecked { i+=1; }  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L205)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 283  
```sol
if (collateralAsset == token0) amtA -= feeAmount;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L283)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 284  
```sol
else amtB -= feeAmount;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L284)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 210  
```sol
fee0 -= added0;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L210)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 211  
```sol
fee1 -= added1;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L211)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 243  
```sol
fee0 += newFee0;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L243)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 244  
```sol
fee1 += newFee1;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L244)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 245  
```sol
n0   -= newFee0;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L245)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 246  
```sol
n1   -= newFee1;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L246)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 317  
```sol
removed0 += fee0 * lp / totalSupply();  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L317)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 318  
```sol
fee0 -= fee0 * lp / totalSupply();  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L318)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 322  
```sol
removed1 += fee1 * lp / totalSupply();  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L322)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 323  
```sol
fee1 -= fee1 * lp / totalSupply();  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L323)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 345  
```sol
amt0 += fee0;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L345)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 346  
```sol
amt1 += fee1;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L346)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 379  
```sol
token0Amount += fee0 * amount / totalSupply();  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L379)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 380  
```sol
token1Amount += fee1 * amount / totalSupply();  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L380)**  

</details>

______
 [**G016] Splitting require() statements that use && saves gas - (saves 8 gas per &&)**

**Impact**  
By using multiple require() statements with one condition per require() statement, you can save 8 gas per &&. The gas difference is only realized if the revert condition is met.  
  
**Recommendation**  
You can split the require() statements to save gas
<details><summary>View Findings (26)</summary>

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 120  
```sol
require(t0 == token0 && t1 == token1, "GEV: Invalid TR");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L120)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 141  
```sol
require(t0 == token0 && t1 == token1, "GEV: Invalid TR");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L141)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 170  
```sol
require(t0 == token0 && t1 == token1, "GEV: Invalid TR");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L170)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 377  
```sol
if (oraclePrice < priceX8 * 101 / 100 && oraclePrice > priceX8 * 99 / 100) matches = true;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L377)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 434  
```sol
if ( (amt0 == 0 && amt0n > 0) || (amt1 == 0 && amt1n > 0) )  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L434)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 167  
```sol
require(options.length == amounts.length && sourceSwap.length == options.length, "OPM: Array Length Mismatch");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L167)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 234  
```sol
if ( repayAmount > 0 && repayAmount < debt ) debt = repayAmount;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L234)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 345  
```sol
(debtValue < 1e8 && tokensValue < 1e8 )  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L345)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 346  
```sol
|| (tokensValue > debtValue * 98 / 100 && tokensValue < debtValue * 102 / 100),  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L346)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 473  
```sol
if (amount > 0 && AmountsRouter(address(ammRouter)).getAmountsOut(amount, path)[1] > 0){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L473)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 495  
```sol
require( amountsIn[0] <= maxAmount && amountsIn[0] > 0, "OPM: Invalid Swap Amounts" );  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L495)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 523  
```sol
require ( priceAssetA > 0 && priceAssetB > 0, "OPM: Invalid Oracle Price");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L523)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 536  
```sol
require(token0 == address(t0) && token1 == address(t1), "OPM: Invalid Debt Asset");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L536)**  

**File:** 2023-08-goodentry/contracts/RangeManager.sol  
**Line Number:** 108  
```sol
require(step < tokenisedRanges.length && step < tokenisedTicker.length, "Invalid step");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RangeManager.sol#L108)**  

**File:** 2023-08-goodentry/contracts/RoeRouter.sol  
**Line Number:** 70  
```sol
&& token0 != address(0x0)  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RoeRouter.sol#L70)**  

**File:** 2023-08-goodentry/contracts/RoeRouter.sol  
**Line Number:** 71  
```sol
&& token1 != address(0x0)  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RoeRouter.sol#L71)**  

**File:** 2023-08-goodentry/contracts/RoeRouter.sol  
**Line Number:** 72  
```sol
&& ammRouter != address(0x0),  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RoeRouter.sol#L72)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 177  
```sol
if ((newFee0 == 0) && (newFee1 == 0)) return;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L177)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 190  
```sol
if ((fee0 * 100 > bal0 ) && (fee1 * 100 > bal1)) {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L190)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 209  
```sol
require(addedValue > liquidityValue * 95 / 100 && liquidityValue > addedValue * 95 / 100, "TR: Claim Fee Slippage");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L209)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 237  
```sol
if ( fee0+fee1 > 0 && ( n0 > 0 || fee0 == 0) && ( n1 > 0 || fee1 == 0 ) ){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L237)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 268  
```sol
if ( newFee0 == 0 && newFee1 == 0 ){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L268)**  

**File:** 2023-08-goodentry/contracts/TokenisableRange.sol  
**Line Number:** 271  
```sol
require (TOKEN0_PRICE > 0 && TOKEN1_PRICE > 0, "Invalid Oracle Price");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/TokenisableRange.sol#L271)**  

**File:** 2023-08-goodentry/contracts/helper/LPOracle.sol  
**Line Number:** 29  
```sol
require(lpToken != address(0x0) && clToken0 != address(0x0) && clToken1 != address(0x0), "Invalid address");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/LPOracle.sol#L29)**  

**File:** 2023-08-goodentry/contracts/helper/LPOracle.sol  
**Line Number:** 101  
```sol
require(decimalsA <= 18 && decimalsB <= 18, "Incorrect tokens");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/LPOracle.sol#L101)**  

**File:** 2023-08-goodentry/contracts/helper/OracleConvert.sol  
**Line Number:** 23  
```sol
require(clToken0 != address(0x0) && clToken1 != address(0x0), "Invalid address");  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/OracleConvert.sol#L23)**  

</details>

______
 [**G017] ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow**

**Impact**  
When using Solidity version 0.8.0 or higher, use the unchecked keyword with ++i or i++ when it is not possible for them to overflow. This can save 30-40 gas per loop.  
  
**Recommendation**  
use the unchecked keyword when it is not possible for the counter to overflow
<details><summary>View Findings (8)</summary>

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 154  
```sol
for (uint k = 0; k < ticks.length - 2; k++)  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L154)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 299  
```sol
for (uint k = 0; k < ticks.length; k++){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L299)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 314  
```sol
for (uint k = 0; k < ticks.length; k++){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L314)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 393  
```sol
for(uint k=0; k<ticks.length; k++){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L393)**  

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 431  
```sol
for (activeTickIndex = 0; activeTickIndex < ticks.length - 3; activeTickIndex++){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L431)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 71  
```sol
for ( uint8 k = 0; k<assets.length; k++){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L71)**  

**File:** 2023-08-goodentry/contracts/PositionManager/OptionsPositionManager.sol  
**Line Number:** 93  
```sol
for ( uint8 k =0; k<assets.length; k++){  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/PositionManager/OptionsPositionManager.sol#L93)**  

**File:** 2023-08-goodentry/contracts/RangeManager.sol  
**Line Number:** 62  
```sol
for (uint i = 0; i < len; i++) {  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RangeManager.sol#L62)**  

</details>

______
 [**G022] Using private for constants saves gas**

**Impact**  
Using public or internal for constants may increase deployment gas costs due to the compiler's necessity to create non-payable getter functions. Additionally, it requires storing the bytes of the value outside of where it's used and adding another entry to the method ID table.  
  
**Recommendation**  
To optimize gas, consider declaring constants with the `private` visibility.
<details><summary>View Findings (1)</summary>

**File:** 2023-08-goodentry/contracts/GeVault.sol  
**Line Number:** 62  
```sol
uint public constant nearbyRanges = 2;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/GeVault.sol#L62)**  

</details>

______
 [**G023] Using bool for storage incurs overhead**

**Impact**  
Using the `bool` data type for storage can result in extra gas costs, especially when altering the boolean's state multiple times. Specifically, changing a `bool` from `false` to `true` after it was previously set to `true` can incur a `Gsset` charge of 20000 gas. 
  
**Recommendation**  
Consider using `uint256` values such as `uint256(1)` for `true` and `uint256(0)` for `false` to avoid these gas overheads. See [source](#) for more information.
<details><summary>View Findings (2)</summary>

**File:** 2023-08-goodentry/contracts/RoeRouter.sol  
**Line Number:** 28  
```sol
bool isDeprecated;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/RoeRouter.sol#L28)**  

**File:** 2023-08-goodentry/contracts/helper/V3Proxy.sol  
**Line Number:** 65  
```sol
bool acceptPayable;  
```
**[Link to Code](https://github.com/code-423n4/2023-08-goodentry/blob/main/contracts/helper/V3Proxy.sol#L65)**  

</details>

</details>
