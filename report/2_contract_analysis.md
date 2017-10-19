# Contract Analysis

## General Findings
- Consider [timestamp dependence issues](https://github.com/ConsenSys/smart-contract-best-practices#timestamp-dependence) when using `now` as opposed to block number.
- IPFS persistence may be an issue. Files with less demand will go down if hosts go offline. There should be a more stable way to save this data or ensure its persistence since the data is extremely important to the use case. 
- The hash of any given data on chain is not able to be verified 
- Not sybil resistant.
- Remove `^` from pragma statement to concretely set version
- There are no checks for invalid addresses (at the very least != 0)
- Miners can see fulfillments, withhold those transactions, and submit the data for themselves, front running the bounty. 

### Modifiers

##### `onlyOwner`
##### `validateNotTooManyBounties`
##### `validateNotTooManyFulfillments`
##### `validateBountyArrayIndex`
##### `onlyIssuer`
##### `onlyFulfiller`
##### `amountIsNotZero`
- Should this check if it is greater than 0?  
##### `transferredAmountEqualValue`
- Should assert the difference in balance equals the transferred amount
- Put `require(msg.value == 0)` before the `transferFrom` if statement to avoid unnecessary computation

##### `isBeforeDeadline`
- This is often used before checking the validity of the `_bountyId`, which could cause an array out of bounds error

##### `validateDeadline`
##### `isAtStage`
##### `validateFulfillmentArrayIndex`
- Might want to move this up next to other index check modifiers, for code cleanliness 

##### `notYetAccepted`
##### `isNotDead`
##### `notIssueOrArbiter`
##### `fulfillmentNotYetAccepted`
##### `enoughFundsToPay`
- change comparison operator to be consistent with previous comparison in `activateBounty` (always say `x > y` rather than `x <= y`)

##### `checkFulfillmentIsApprovedAndUnpaid`
##### `newDeadlineIsValid`
##### `newFulfillentAmountIsIncrease`

# Functions

### Issuer Functions:

##### `StandardBounties`

##### `issueBounty`
- Verify that the token address is valid
##### `issueandActivateBounty`
- Needs a check on `msg.value == 0` if `_paysTokens == true`
- Verify that the token address is valid

##### `activateBounty`
- Need to call modifier `validateBountyArrayIndex` first, since other modifiers rely on this index of the array holding a non-null value
- Question: Is reactivating dead bounties intended functionality? 

##### `acceptFulfillment`

##### `killBounty`
- Issuer can drain all the money without refunding any contributors. This disincentivizes contributors from participating in the bounty.
- Best practice to have `difference > 0` in the case that the EVM/solc converts `uint` types into `int` for comparison. Also better for good practice and clarity. 

##### `extendDeadline`
##### `transferIssuer`
- This function does not check if `_newIssuer` is a valid address. An issuer could greif attack the bounty by transferring to the `0x0` address. Now the contract is stuck in that same state and no one can remove their money.
- Plausible deniability for malicious issuer. Issuers can steal all data, swap issuer, say that the new issuer was the malicious one (even when owned by same person). 
- Blame for malicious activity can be directed to the new issuer, B as long as the original issuer, A claims to have been attacked/decieved by B. 
##### `changeBountyDeadline`
- `@dev` comment is incorrect for the function

##### `changeBountyData`
- `@dev` comment is incorrect for the function

##### `changeBountyFulfillmentAmount`
- `@dev` comment is incorrect for the function

##### `changeBountyArbiter`
- `@dev` comment is incorrect for the function

##### `changeBountyPaysTokens`
- By changing the bounty payout token type, the issuer can drain the contract of all funds, including the contributors. So the issuer can steal all the contributors funds with or without intending to do so, only intending to change the payout type of the contract.
- Should check `oldPaysTokens` and `_newPaysTokens` if `tokenContract[_boundyId]` already points to a token, to see if the bounty is transitioning from some `HumanStandardToken` to ether. In this case, the `_newTokenContract` parameter should not be considered and the previous `tokenContract[_bountyId]` address should be deleted.
- If the bounty is transitioning from ether to a token, there should be a check to make sure the new token address is legitimate.

### Public/Fulfiller Functions

##### `increasePayout`

##### `contribute`
- Need to call modifier `validateBountyArrayIndex` first, since other modifiers rely on this index of the array holding a non-null value

##### `fulfillBounty`
##### `updateFulfillment`

##### `fulfillmentPayment`


### Constant Functions

##### `getFulfillment`
##### `getBounty`
##### `getBountyArbiter`
##### `getBountyData`
##### `getBountyToken`
##### `getNumBounties`
##### `getNumFulfillments`

### Internal Functions
##### `transitionToState`
