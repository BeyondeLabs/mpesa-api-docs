## 5.1 Request Party Matrix

Command Types | InitiatorLinkedTo | Primary Party | Receiver Party
----------------------- | ---------------------- | ------------------ | ------------------
`BusinessBuyGoods` | C2B, B2C, Merchant HO and Store | Identity – C2B, B2C, Merchant HO and Store <br/><br/>IdentifierType - 4<br/><br/>Identifier - ShortCode | Identity - Merchant HO and Store<br/><br/>IdentifierType - 4<br/><br/>Identifier - ShortCode
`BusinessPayBill` | C2B, B2C, Merchant HO and Store | Identity - C2B, B2C, Merchant HO and Store<br/><br/>IdentifierType - 4<br/><br/>Identifier - ShortCode | Identity - C2B Business<br/><br>IdentifierType - 4<br/><br/>Identifier - ShortCode
`DisburseFundsToBusiness` | B2C and C2B Business | Identity –C2B and B2C<br/>IdentifierType - 4<br/><br/>Identifier – ShortCode | Identity - C2B, B2C, Merchant HO and Store<br/><br/>IdentifierType - 4<br/><br/>Identifier - ShortCode
`BusinessToBusinessTransfer` | C2B, B2C, Merchant HO and Store, Agency HO and store | Identity – C2B, B2C, Merchant HO and Store <br/><br/>IdentifierType - 4 <br/><br/>Identifier – ShortCode | Identity - C2B, B2C, Merchant HO and Store <br/><br/>IdentifierType - 4 <br/><br/>Identifier - ShortCode
`BusinessTransferFromMMFToUtility` | C2B, B2C | Identity – C2B and B2C <br/><br/>IdentifierType - 4 <br/><br/>Identifier – ShortCode | Same as Primary Party
`BusinessTransferFromUtilityToMMF` | C2B, B2C | Identity – C2B and B2C <br/><br/>IdentifierType - Merchant HO and Store <br/><br/>IdentifierType – 4 <br/><br/>Identifier – ShortCode | Same as Primary Party
`MerchantTransferFromMerchantToWorking` | Merchant HO and Store | Identity – Merchant HO and Store <br/><br/>IdentifierType - 4 <br/><br/>Identifier – ShortCode | Same as Primary Party
`MerchantTransferFromWorkingToMerchant` | Merchant HO and Store | Identity – Merchant HO and Store <br/><br/>IdentifierType - 4 <br/><br/>Identifier – ShortCode | Same as Primary Party
`MerchantToMerchantTransfer` | Merchant HO and Store (Within Hierarchy). The short code must be the Store shortcode or HO shortcode | Identity – Merchant HO and Store <br/><br/>IdentifierType - 4 <br/><br/>Identifier – ShortCode | Identity – Merchant HO and Store <br/><br/>IdentifierType - 4 <br/><br/>Identifier – ShortCode
`MerchantServicesMMFAccountTransfer` | Merchant HO and Store (Within Hierarchy). The short code must be the Store shortcode or HO shortcode | Identity – Merchant HO and Store <br/><br/>IdentifierType - 4 <br/><br/>Identifier – ShortCode | Identity – Merchant HO and Store <br/><br/>IdentifierType - 4 <br/><br/>Identifier – ShortCode
`AgencyFloatAdvance` | Agency HO and Store | Identity – Merchant HO and Store <br/><br/>IdentifierType - 4 <br/><br/>Identifier – ShortCode | Identity – Merchant HO and Store <br/><br/>IdentifierType - 4 <br/><br/>Identifier – ShortCode
`OrgBankAccountWithdrawal` | C2B, B2C, Merchant HO and Store | Identity – C2B, B2C, Merchant HO and Store <br/><br/>IdentifierType - 4 <br/><br/>Identifier – ShortCode | Same as Primary Party
`OrgRevenueSettlement` | C2B, B2C, Merchant HO and Store | Identity – C2B, B2C, Merchant HO and Store <br/><br/>IdentifierType - 4 <br/><br/>Identifier – ShortCode | Same as Primary Party
`AccountBalance` | B2C Business, C2B Business, Merchant HO, MMF Organisation | NA (0) | NA (0)
`TransactionStatusQuery` | B2C Business, C2B Business, Merchant HO, MMF Organisation | NA (0) | NA (0)

## 5.2 Business Paybill

### Description
This is a transaction type that moves money from Business customer (B2C, C2B, Merchant HO and Merchant Store organizations) to a C2B organization. The request moves money from the Primary Party’s MMF/WORKING account to the Receiver party’s Utility account. Primary party will be API Enabled Business Customer and receiver party will be C2B Business. The initiator would be linked to primary party.

### Input Parameters

Parameter | Type | Description
----------- | ------------- | ---------------
Amount | Decimal | |
AccountReference | String | The third party account reference. (Mandatory) | 

### Output Parameters

Parameter | Type | Description
----------- | ------------- | ---------------
`ResultType` | ResultType | It describes the state of the request, for single staged it would be only completed.
`ResultCode` | String | Result code
`ResultDesc` | String | Result description
`ConversationId` | GUID | M-Pesa conversation id
`OriginatorConversationId` | String | Third party conversation id
`TransactionId` | String | It takes the financial transaction unique receipt number.
`Amount` | Decimal | The transactional Amount <br/> e.g. `<Key>Amount</Key><Value>7000.00</Value>`
`TransCompletedTime` | DateTime | The date time when the transaction was completed. <br/> e.g. `<Key>TransCompletedTime</Key> <Value>20150424170252</Value>`
`InitiatorAccountCurrentBalance` | Decimal | Amount of money in the initiating organization. <br/> e.g. `<Key>InitiatorAccountCurrentBalance</Key><Value>{Amount={BasicAmount=2714803.00, MinimumAmount=271480300, CurrencyCode=KES}}</Value>`
`DebitPartyCharges` | Decimal | |
`DebitAccountCurrentBalance` | Decimal | Amount of money in the initiating organization.<br/> e.g. `<Key> DebitAccountCurrentBalance </Key><Value>{Amount={BasicAmount=2714803.00, MinimumAmount=271480300, CurrencyCode=KES}}</Value>`
`Currency` | String | Currency used. <br/> e.g. `<Key>Currency</Key> <Value>KES</Value>`
`DebitPartyPublicName` | String | Public name of the Credit Party. <br/> e.g. `<Key>ReceiverPartyPublicName</Key> <Value>888888 - KPLC TEST</Value>`
`DebitPartyAffectedAccountBalance` | String | Indicates the account balance of all the affected accounts of debit party. <br/>For each account, the fields are presented in the following order and separated by vertical bars (`\|`):  & is used as the separator of different accounts.<br/> e.g. `<Key>DebitPartyAffectedAccountBalance</Key><Value>MMF Organization Account\|KES\|2714803.00\|2714803.00\|0.00\|50000000.00</Value>`




