# SecuritiesSettlementEngine

## Introduction

SecuritiesSettlementEngine represents a Proof of Concept (POC) securities settlement engine built on Ethereum. The project is composed of:

- Definition of an Ethereum-based bond
- Four types of instructions: SecurityIssuance, DvP, RvP, SecurityRedemption
- Roles involved in the issuance, custodianship, and redemption (CSD, IPA, Issuer, Dealer) of a security
- A matching engine and settlement engine with a built-in 4-eyes principle.

## Requirements

### Roles

1. **Central Securities Depository (CSD)**
   - **Role**: The CSD acts as the central hub that operates the matching and settlement engine for bond transactions. It maintains a list of client addresses mapped to specific roles, such as IPA, Issuer, and Dealers. The CSD’s primary function is to create and manage various financial instructions on behalf of its clients, such as SecurityIssuanceInstructions, Delivery Versus Payment (DVP) instructions, Receive Versus Payment (RVP) instructions, and Redemption instructions.
   - **Allowed Actions**:
     - Create instructions on behalf of clients.
     - Maintain and manage the roles and permissions of its clients.
     - Match and settle transactions according to the instructions provided by the clients.
   - **Restrictions**:
     - The CSD itself cannot initiate or participate directly in any financial transactions like security issuances, DVP, RVP, or redemptions.
     - It does not act as a custodian for its clients' assets, meaning it does not hold or manage the securities or funds of its clients.

2. **Issuing Paying Agent (IPA)**
   - **Role**: The IPA serves as the intermediary between the Issuer and the investors (or Dealers). Its primary responsibility is to handle the financial transactions associated with bond issuance and redemption on behalf of its investors. The IPA requests SecurityIssuanceInstructions from the CSD and facilitates the flow of capital from investors to the Issuer and the repayment from the Issuer to the investors.
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

4. **Dealers**
   - **Role**: Dealers act as representatives of the bond's investors, typically holding partial ownership of the bonds. They can engage in the purchase of bonds from the Issuer and may initiate SecurityIssuanceInstructions. However, any issuance initiated by a Dealer requires authorization from the IPA. Dealers are crucial in the distribution and trading of bonds in the market.
   - **Allowed Actions**:
     - Initiate SecurityIssuanceInstructions with the necessary IPA authorization.
     - Engage in DVP, RVP, issuance, or redemption transactions, representing the investors’ interests.
   - **Restrictions**:
     - Cannot finalize a SecurityIssuanceInstruction without the IPA’s authorization.
     - For this proof of concept (POC), only one Dealer is allowed to participate in each DVP, RVP, issuance, or redemption instruction.

### Summary

In this system, the CSD operates as the central administrator and facilitator of bond transactions, while the IPA acts as a financial intermediary between the Issuer and Dealers (representing investors). The Issuer raises funds through bond issuance, and the Dealers represent the investors who ultimately hold the bond securities. The roles are defined to ensure a clear separation of responsibilities and to prevent any single entity from monopolizing the transaction process.

## Future Work

- (Detail any potential expansions or additional features planned for the SecuritiesSettlementEngine.)
