### Ownership can be lost if mistakenly call **renounceOwnership** function in Locking.sol contract. 

## Summery

Locking contracting is inheriting abstract Ownable contract from "@openzeppelin/contracts/access/Ownable.sol". Ownable contract has a function named **renounceOwnership** which transfer ownership to zero address.

```solidity

     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby disabling any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }
```

### Impact

if the current owner mistakenly call **renounceOwnership** function then the ownership will transfered to address(0).

### Proposed Solution

Override the **renounceOwnership** function in Locking contract with proper logic to prevent ownership transfer to address(0).

```solidity 

    function renounceOwnership() public virtual onlyOwner {
        revert("Locking: Ownership not transferable to zero address");
    }
```

