## 4.1 Interface: `RequestMgrPortType`

### 4.1.1 Operation: `GenericAPIRequest`
The 3rd party invokes this operation to send a B2B request.

#### 4.1.1.1 Message Header: `RequestSOAPHeader`

Element name | Element type | Optional | Description
------------ | ------------ | -------- | ------------
SpId | `xsd: string` | No | SP ID. This is the Service Provider Identifier that is allocated by the Broker to the 3rd party. [Example] 000201
SpPassword | `xsd: string` | Yes | <p>This is an encrypted form of the SP password issued to an SP when an account is created on the Broker.</p><p>The encrypted password is a Base64 encoded string of the SHA- 256 hash of the concatenation of the spId, password and the timeStamp as illustrated below:</p><p>Given the following parameters:<br/>spId: 601399 <br/>password: spPassword <br/>timestamp: 20130702212854</p><p>spPassword = BASE64(SHA-256(spId + Password + timeStamp)), e.g. <br/>`spPassword = BASE64(SHA- 256(601399spPassword20130702212854)`<br/>[Example] `e6434ef249df55c7a21a0b45758a39bb`</p>
ServiceId | `xsd: string` | Yes | <p>Service ID.<br/>This is the Service Identifier that is allocated by the Broker for every service created.<br/>[Example] 3500001000012</p>
Timestamp | `xsd: string` | Yes | <p>Time stamp (UTC time).<br/>The value is required during SHA-256 encryption for spPassword.<br/>NOTE: If the `spPassword` parameter must be set, this parameter is mandatory.<br/>[Format] yyyyMMddHHmmss <br/>[Example] 20100731064245</p>

#### 4.1.1.2 Input Message: `RequestMsg`

Element name | Element type | Optional | Description
------------ | ------------ | -------- | ------------
RequestMsg | `xsd: string` | No | Request Message from 3rd party. Its value should be an instance of Request Type and a CDATA

**Note:**

-  If there is no configuration for notification URL on Broker side, which indicates the callback url for accepting notification of GenericAPIResult, the `ResultURL` parameter inside Identity tag must present.
- If there is no configuration for notification URL on Broker side, which indicates the callback url for accepting notification of cached requests expired, the 3rd party must add a key-pair parameter into `ReferenceData` and the key is `QueueTimeoutURL`.

#### 4.1.1.3 Output Message: `ResponseMsg`

Element name | Element type | Optional | Description
------------ | ------------ | -------- | ------------
ResponseMsg | `xsd: string` | No | Response return to 3rd party. Its value should be an instance of Response Type and a CDATA.

## 4.2 Interface: `ResultMgrPortType`

### 4.2.1 Operation: `GenericAPIResult`

This operation must be implemented by a Web Service at the 3rd party side if it requires notification of the final result for B2B request. It will be invoked by Broker to notify the 3rd party once Broker received the notification from Core API.

#### 4.2.1.1 Input Message: `ResultMsg`

Element name | Element type | Optional | Description
------------ | ------------ | -------- | ------------
ResultMsg | `xsd: string` | No | Request Message from Broker. Its value should be a instance of Result Type and a CDATA.

#### 4.2.1.2 Output Message: ResponseMsg

Element name | Element type | Optional | Description
------------ | ------------ | -------- | ------------
ResponseMsg | `xsd: string` | No | Response return to Broker. Its value should be a instance of Response Type and a CDATA.

## 4.3 Interface: `QueueTimeoutNotificationPort`

### 4.3.1 Operation: `notifyQueueTimeout`

This operation must be implemented by a Web Service at the 3rd party side if it requires notification of cached requests are expired. It will be invoked by Broker to notify the 3rd party once cached requests are expired.

#### 4.3.1.1 Input Message: `notifyQueueTimeout`

Element name | Element type | Optional | Description
------------ | ------------ | -------- | ------------
OriginatorConversationID | `xsd:string` | | originatorConversationID from the request sent by the 3rd party
originRequest | `xsd:string` | No | Original request without SOAP Header sent by 3rd party. Its value is encoded with base64, when the 3rd party receive the request, it should decode it.
ExtensionInfo | Parameters | Yes | Extended parameters.

#### 4.3.1.2 Output Message: notifyQueueTimeoutResponse

Element name | Element type | Optional | Description
------------ | ------------ | -------- | ------------
Result | Result | Result | No | 
ExtensionInfo | Parameters | Yes | Extended parameters.

#### 4.3.1.3 Response Code

ResponseCode | ResponseDesc
------------ | ------------
000000000 | Success
000000001 | Failed

## 4.4 Interface: `QueryTransactionPort`

### 4.4.1 Operation: `queryTransaction`

The 3rd party invokes this operation to query transaction information.

#### 4.4.1.1 Message Header: `RequestSOAPHeader`

Element name | Element type | Optional | Description
------------ | ------------ | -------- | ------------
SpId | `xsd: string` | No | <p>SP ID.<br/>It’s allocated by the Broker to the 3rd party.<br/>[Example] 000201</p>
spPassword | `xsd: string` | Yes | <p>Encrypted authentication password for partners to access the Broker.</p><p>The value is a character string encrypted from `spId + Password + timeStamp` by SHA-256. The encryption formula is as follows: spPassword =BASE64(SHA-256(spId + Password + timeStamp))</p><p>In the preceding formula: timeStamp: value of timeStamp.Password: authentication password for 3rd parties to access the Broker. The value is allocated by the Broker.</p><p>**NOTE:**<br/>The authentication modes include SPID&Password, SPID&IP&Password, and SPID&IP. When the authentication mode is SPID&Password or SPID&IP&Password, this parameter is mandatory.<br/>[Example] e6434ef249df55c7a21a0b45758a39bb</p>
ServiceId | `xsd: string` | Yes | <p>Service ID.<br/>The value is allocated by the Broker to the 3rd party.<br/>[Example] 3500001000012</p>
timeStamp | `xsd: string` | Yes | <p>Time stamp (UTC time).<br/>The value is required during SHA-256 encryption for spPassword.</p><p>**NOTE:**<br/>If the spPassword parameter must be set, this parameter is mandatory.<br/>[Format] yyyyMMddHHmmss [Example] 20100731064245</p>

#### 4.4.1.2 Input Message: `queryTransaction`

Element name | Element type | Optional | Description
------------ | ------------ | -------- | ------------
originatorConversationID | `xsd:string` | | The unique identifier of the request message generated by third party. It is used to identify a request between the third party and MM. Max length is 128
extensionInfo | Parameters | Yes | Extended parameters.

**Table 4-1** extensionInfo Description

Parameter | Optional | Type | Description
--------- | -------- | ----- | --------------
queryDate | Yes | String(20) | <p>The date of the original conversation. Format is yyyyMMddHHmmss, for example: 20131230134412</p><p>Note: <br/>If this parameter does not present, it will cost more time to get the result.</p>

#### 4.4.1.3 Output Message: `queryTransactionResponse`

Element name | Element type | Optional | Description
------------ | ------------ | -------- | ------------
Result | Response | No |
submitApiRequestList | `xsd:string[0-unbounded]` | Yes | Requests sent by the 3rd party. Its value is the requests sent by the 3rd party with base64 encoded.
submitApiResponseList | `xsd:string[0-unbounded]` | Yes | Responses returned from the Broker. Its value is the responses returned from the Broker with base64 encoded.
submitApiResultList | `xsd:string[0-unbounded]` | Yes | Results sent to the 3rd party. Its value is the requests sent by the Broker with base64 encoded.
queueTimeOutList | `xsd:string[0-unbounded]` | Yes | QueueTimeout requests sent to the 3rd party. Its value is the requests sent by the Broker with base64 encoded.
extensionInfo | Parameters | Yes | Extended parameters.

#### 4.4.1.4 Response Codes

ResponseCode | ResponseDesc
------------ | -----------------
000000000 | Success
100000001 | The system is overload
100000002 | Throttling error
100000003 | Exceed the limitation of the LICENSE
100000004 | Internal Server Error
100000005 | <p>Invalid input value:%1<br/>%1 indicates the parameter’s name.</p>
100000006 | SP’s status is abnormal
100000007 | Authentication failed
100000008 | Service’s status is abnormal
100000010 | Insufficient permissions
100000014 | <p>Missing mandatory parameter:%1 <br/>%1 indicates the parameter’s name.</p>

## 4.5 Interface: `Management`

### 4.5.1 Operation: `changePassword`

The 3rd party invokes this operation to change his password.

### 4.5.2 Input Message: `changePassword`

Element name | Element type | Optional | Description
------------ | ------------ | -------- | ------------
spId | `xsd: string` | No | <p>SP ID.<br/>It’s allocated by the SAG to the 3rd party. <br/>[Example] 000201</p>
spPassword | `xsd: string` | Yes | <p>Encrypted authentication password for partners to access the SAG.</p><p>The value is a character string encrypted from `spId + Password + timeStamp` by SHA-256. The encryption formula is as follows: spPassword =BASE64(SHA- 256(spId + Password + timeStamp))</p><p>In the preceding formula:<br/>`timeStamp:` value of timeStamp.<br/>`Password:` authentication password for 3rd parties to access the SAG. The value is allocated by the SAG.</p><p>**NOTE**:<br/>The authentication modes include SPID&Password, SPID&IP&Password, and SPID&IP. When the authentication mode is SPID&Password or SPID&IP&Password, this parameter is mandatory.</p><p>[Example] e6434ef249df55c7a21a0b45758a39bb</p>

### 4.5.3 Output Message: `changePasswordResponse`

Element name | Element type | Optional | Description
------------ | ------------ | -------- | ------------
result | Result | No | Result
extensionInfo | Parameters | Yes | Extended parameters

### 4.5.4 Response Code

ResponseCode | ResponseDesc
------------ | ------------------
000000000 | Success
100000001 | The system is overload
100000002 | Throttling error
100000003 | Exceed the limitation of the LICENSE
100000004 | Internal Server Error
100000005 | Invalid input value:%1<br/>%1 indicates the parameter’s name.
100000006 | SP’s status is abnormal
100000007 | Authentication failed
100000014 | Missing mandatory parameter:%1 <br/>%1 indicates the parameter’s name.







