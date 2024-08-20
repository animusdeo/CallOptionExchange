# SecuritiesSettlementEngine

## Introduction

SecuritiesSettlementEngine represents a Proof of Concept (POC) securities settlement engine built on Ethereum. The project is composed of:

- Definition of an Ethereum-based bond
- Four types of instructions: SecurityIssuance, and SecurityRedemption
- Roles involved in the issuance, custodianship, and redemption (CSD, IPA, Issuer) of a security
- A matching engine and settlement engine with NO built-in 4-eyes principle.

## Requirements

### Roles

1. **Central Securities Depository (CSD)**
   - **Role**: The CSD acts as the central hub that operates the matching and settlement engine for bond transactions. It maintains a list of client addresses mapped to specific roles, such as IPA, Issuer, and Dealer. The CSD’s primary function is to create and manage various financial instructions on behalf of its clients, such as SecurityIssuanceInstructions and Redemption instructions.
   - **Allowed Actions**:
     - Create instructions on behalf of clients.
     - Maintain and manage the roles and permissions of its clients.
     - Match and settle transactions according to the instructions provided by the clients.
   - **Restrictions**:
     - The CSD itself cannot initiate or participate directly in any financial transactions like security issuances or redemptions.
     - It does not act as a custodian for its clients' assets, meaning it does not hold or manage the securities or funds of its clients.

2. **Issuing Paying Agent (IPA)**
   - **Role**: The IPA serves as the intermediary between the Issuer and the investors. Its primary responsibility is to handle the financial transactions associated with bond issuance and redemption on behalf of its investors. The IPA requests SecurityIssuanceInstructions from the CSD and facilitates the flow of capital from investors to the Issuer and the repayment from the Issuer to the investors.
   - **Allowed Actions**:
     - Request and initiate SecurityIssuanceInstructions from the CSD.
     - Deliver investor funds to the Issuer in exchange for bonds.
     - Receive the Issuance Outstanding Amount (IOA) from the Issuer upon the bond's settlement or maturity.
     - Authorize SecurityIssuanceInstructions initiated by Dealers.
   - **Restrictions**:
     - Cannot create instructions independently of the CSD; must act through the CSD for all instruction-related actions.

3. **Issuer**
   - **Role**: The Issuer is the entity seeking to raise capital through the issuance of bonds. The Issuer sells debt securities to the IPA in exchange for capital. The Issuer is responsible for delivering the bond to the IPA, representing the debt, and for repaying the Issuance Outstanding Amount (IOA) on the settlement date. The Issuer typically interacts with the IPA and is obligated to repay the bond's principal at maturity.
   - **Allowed Actions**:
     - Issue bonds through SecurityIssuanceInstructions.
     - Receive funds from the IPA in exchange for the bonds.
     - Pay the IOA to the IPA on the bond's settlement or maturity date.
   - **Restrictions**:
     - The Issuer must interact with the IPA for all transactions and cannot bypass this intermediary.

4. **Dealers (future work)**
   - **Role**: Dealers act as representatives of the bond's investors, typically holding partial ownership of the bonds. They can engage in the purchase of bonds from the Issuer and may initiate SecurityIssuanceInstructions. However, any issuance initiated by a Dealer requires authorization from the IPA. Dealers are crucial in the distribution and trading of bonds in the market.
   - **Allowed Actions**:
     - Initiate SecurityIssuanceInstructions with the necessary IPA authorization.
     - Engage in DVP, RVP, issuance, or redemption transactions, representing the investors’ interests.
   - **Restrictions**:
     - Cannot finalize a SecurityIssuanceInstruction without the IPA’s authorization.
     - For this proof of concept (POC), only one Dealer is allowed to participate in each DVP, RVP, issuance, or redemption instruction.
    
### EthereumSecurity

An EthereumSecurity is a contract that represents debt owed by the Issuer of the security towards its holder (e.g. the IPA). The Issuer raises capital by borrowing money from investors (represented by the IPA) and pays a fixed interest to the holder. On the maturity date, the Issuer must repay the debt owed in full. 


### Instructions

Every Instruction has the following members:
   - instructionId
   - processingStatus
      - PENDING_CONFIRMATION
      - PENDING_MATCHING
      - MATCHED
      - PENDING_SETTLEMENT
      - FAILED_SETTLEMENT
      - ATTEMPTING_SETTLEMENT
      - SETTLED
All instruction contracts are created by the CSD on behalf of its clients.
    
In the future, an instruction should have authorisation stages (ACTIVE, CANCELLED, CREATOR_PROPOSED_CANCELLATION) and should be preceeded by proposals e.g. SecurityIssuanceProposal requests. Additionally, all requests should abide by the 4-eyes principle, meaning a second party trusted by the initiating party should approve the request. Last, an instruction should be able to be cancelled using a cancellation instruction.

1. **SecurityIssuanceInstruction**
   - A contract between an Issuer and IPA facilitated by the CSD to issue an EthereumSecurity.
   - Must be created by the Issuer and confirmed by the IPA.
   - After confirmation, the CSD will start the settlement phase. The CSD will spend ETH on behalf of the IPA and the Issuer will receive the total amount. A EthereumSecurity contract is deployed with the IPA's address as owner the represent the debt owed by the Issuer.

2. **SecurityRedemptionInstruction**
   - A contract between an Issuer and IPA holding an EthereumSecurity issued by the Issuer to fully (!) redeem the Total Outstanding Issuance amount.
   - Must be created by the Issuer and confirmed by the IPA.
   - After confirmation, the CSD settles the instruction. The CSD will spend ETH on behalf of the Issuer and the IPA will receive the total amount. An EthereumSecurity that is fully redeemed will be destroyed to represent the full repayment of debt owed by the Issuer.

(Future work)

3. **Delivery versus Payment**
   - An instruction created by the seller of an EthereumSecurity representing its intent to deliver an EthereumSecurity in return for receiving a payment.
   - A DvP represents the former half of a transaction to trade an EthereumSecurity and will only result in a swap if there is a matching RvP created by the recipient of the security.

4. **Receive versus Payment**
   - An instruction created by the buyer of an EthereumSecurity representing its intent to receive an EthereumSecurity in return for delivering a payment
   - A RvP represents the latter half of a transaction to trade an EthereumSecurity and will only result in a swap if there is a matching DvP created by the deliveree of the security.

### Summary

In this system, the CSD operates as the central administrator and facilitator of bond transactions, while the IPA acts as a financial intermediary between the Issuer and Dealers (representing investors). The Issuer raises funds through bond issuance, and the Dealers represent the investors who ultimately hold the bond securities. The roles are defined to ensure a clear separation of responsibilities and to prevent any single entity from monopolizing the transaction process.

## Future Work

- (Detail any potential expansions or additional features planned for the SecuritiesSettlementEngine.)
