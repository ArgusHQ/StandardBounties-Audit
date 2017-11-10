
# Summary of the Process and Scope of the Audit

## The Process

The Blockchain at Berkeley team was contracted the audit by Consensys on October 4, 2017. The audit was submitted to the team on October 19, 2017.

The review process was broken up into an individual pass by each member of the audit team, followed by a pass by all members involved with the audit.

An initial draft was completed on October 12, 2017 and was finalized on October 18, 2017.

## The Scope

The Blockchain at Berkeley team reviewed the StandardBounties code with the following metrics in mind:

### Security 
Identification of potential security flaws/attacks including but not limited to: 
* Reentrancy attacks
* Sybil attacks
* Front running and transaction-ordering dependence
* Call depth attacks
* Data compliance
* Integer Overflow and Underflow
* Race conditions
* DoS attacks
* Timestamp dependence


### Code Quality
Review of code to follow general best practices and [Consensys Best Practices](https://github.com/ConsenSys/smart-contract-best-practices).

Focal Points:
* Syntax
* Readability
* Consistency
* Complexity
* Scalability

### Architecture
Reivew of the smart contract architecture with primary focus on:
* Game theory
* Incentives
* Pain Points
* Usability
