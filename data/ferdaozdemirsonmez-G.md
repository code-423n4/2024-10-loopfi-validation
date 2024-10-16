## Title
Gas Limit Risk in Token Loop in RewardManager.sol

## Issue
The loop in _doTransferOutRewards() processes every token in tokens[], which could lead to exceeding the block gas limit if the array becomes too large.

## Proof of Concept

https://github.com/code-423n4/2024-10-loopfi/blob/d219f0132005b00a68f505edc22b34f9a8b49766/src/pendle-rewards/RewardManager.sol#L63

https://github.com/code-423n4/2024-10-loopfi/blob/d219f0132005b00a68f505edc22b34f9a8b49766/src/pendle-rewards/RewardManager.sol#L84

The loop iterates over each token:

for (uint256 i = 0; i < tokens.length; i++) {
    rewardAmounts[i] = userReward[tokens[i]][user].accrued;
    if (rewardAmounts[i] != 0) {
        userReward[tokens[i]][user].accrued = 0;
        rewardState[tokens[i]].lastBalance -= rewardAmounts[i].Uint128();
    }
}
This could fail with many tokens, leading to DoS from gas exhaustion.

## Mitigation
-Use batch processing to reduce gas usage, or cap the number of tokens processed at once.

## Risk Level
Medium