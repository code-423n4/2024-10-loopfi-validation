## GA-1
Check if both `deltaCollateral` and `deltaDebt` are zero. If there are no changes to be made, you can revert early to save gas and prevent unnecessary execution.

```
if (deltaCollateral == 0 && deltaDebt == 0) {
    revert CDPVault__modifyCollateralAndDebt_noChanges(); 
}
```
affected;
https://github.com/code-423n4/2024-10-loopfi/blob/d219f0132005b00a68f505edc22b34f9a8b49766/src/CDPVault.sol#L429

## GA-2
## Zero Repayment Amount Wastes Gas
Repaying zero tokens doesn't make sense and wastes gas. Revert the transaction if `amount` is 0.

```
if (amount == 0) {
    revert CDPVault__repay_zeroAmount();
}
```

affected: https://github.com/code-423n4/2024-10-loopfi/blob/d219f0132005b00a68f505edc22b34f9a8b49766/src/CDPVault.sol#L292
