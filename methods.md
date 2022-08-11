# API Methods and Types

##Table of contents
1. [Pay](#pay)
2. [Reverse payment](#reverse-payment)
3. [Get payment status](#get-payment-status)
4. [Send verification code](#send-verification-code)
5. [Verify verification code](#verify-verification-code)
6. [PreAuthorize](#preauthorize)
7. [Get user information](#get-user-information)
	1. [Get user information v1](#get-user-information-v1)
	2. 	[Get user information v2](#get-user-information-v2)
	3. 	[Get user information v3](#get-user-information-v3)
	4. 	[Get user information summary](#get-user-information-summary)
8. [Direct Payment Fault](#directpaymentfault)

## Pay

Use the Pay method to directly charge a mobile subscription with a provided amount. It is also mandatory to specify a [service code](service-codes.md) for a transaction that describes the category and in turn will effect the payout. The mobile subscription is charged immediately and you will get a synchronous response from the API. It is not necessary for the mobile phone to be turned on in order for the transaction to go through.

Note that some of our services requires the mobile phone subscription to be verified before use - see [here](methods.md#send-verification-code) for details.

Also please note that currently the following norwegian mobile network operators are supported through the API: Telenor, Telio (former Netcom) and Network Norway. Once the other norwegian mobile network operators start supporting Direct Payment, they will be automatically available for you through this API.

### Request types

#### PaymentRequest
<table>
<tr><th>Name</th><th>Data Type</th><th>Description</th><th>Mandatory</th></tr>
<tr><td>ServiceCredentials</td><td>ServiceCredentials</td><td>For authentication purposes. <br><br>
This parameter is only relevant when using the SOAP endpoint. <br><br>
The REST interface gets the ServiceId from the request URL and Username/Password from the Authorization HTTP header.
</td><td>Yes</td></tr>
<tr><td>PaymentDetails</td><td>PaymentDetails</td><td>Contains details about the payment.</td><td>Yes</td></tr>
</table>

#### ServiceCredentials
This type is only relevant when using the SOAP endpoint.

<table>
<tr><th>Name</th><th>Data Type</th><th>Description</th><th>Mandatory</th></tr>
<tr><td>ServiceId</td><td>Integer</td><td>Service ID, provided by Puzzel service desk.</td><td>Yes</td></tr>
<tr><td>Username</td><td>String</td><td>Username, provided by Puzzel service desk.</td><td>Yes</td></tr>
<tr><td>Password</td><td>String</td><td>Password, provided by Puzzel service desk.</td><td>Yes</td></tr>
</table>


#### PaymentDetails
<table>
<tr><th>Name</th><th>Data Type</th><th>Description</th><th>Mandatory</th><th>Version</th></tr>
<tr><td>Msisdn</td><td>String</td><td>The MSISDN that should be charged. The format should follow the ITU-T E.164 standard with a + prefix.</td><td>Yes</td><td>1, 2</td></tr>
<tr><td>Price</td><td>Integer</td><td>The amount that should be debited, in lowest monetary unit. Example: 100 (1,- NOK).</td><td>Yes</td><td>1, 2</td></tr>
<tr><td>ServiceCode</td><td>String</td><td>Service code identifying the type of transaction. See <a href="service-codes.md">here</a> for a list of available service codes</td><td>Yes</td><td>1, 2</td></tr>
<tr><td>ClientReference</td><td>String</td><td>Client reference for the transaction, must be unique.</td><td>Yes</td><td>1, 2</td></tr>
<tr><td>EndUserInvoiceText</td><td>String</td><td>Text that will be displayed on the end-user invoice.</td><td>No</td><td>1, 2</td></tr>
<tr><td>Age</td><td>Integer</td><td>The minimum age of the subscriber required for the purchase. If set, the valid values are 16 and 18.</td><td>No</td><td>1, 2</td></tr>
<tr><td>Differentiator</td><td>String</td><td>Arbitrary string set by the client to enable grouping of messages in certain statistics reports.</td><td>No</td><td>1, 2</td></tr>
<tr><td>InvoiceNode</td><td>String</td><td>Arbitrary string set by the client to enable grouping of messages on the service invoice.</td><td>No</td><td>1, 2</td></tr>
<tr><td>BusinessModel</td><td>String</td><td>Business model, a list containing the available business models is available <a href="business-models.md">here</a>.</td><td>No</td><td>1, 2</td></tr>
<tr><td>AuthorizationToken</td><td>String</td><td>If the user has preauthorized the transaction, you can input the preauth token here. See [here](methods.md#preauthorize) for details.</td><td>No</td><td>2</td></tr>
<tr><td>SecurityLevel</td><td>Enum</td><td>Which security level the authorization request should use.<br><br>
1 = None <br>
2 = Confirmation (End user will have 1 minute to confirm the transaction)<br>
3 = Pin (not in use)</td><td>Yes</td><td>2</td></tr>
<tr><td>ConfirmationChannel</td><td>Enum</td><td>If security level is set to 'Confirmation', you can specify which channel the confirmation message should use.<br><br>
1 = USSD <br>
2 = SMS</td><td>No</td><td>2</td></tr>
</table>


### Response types

#### PaymentResponse
<table>
<tr><th>Name</th><th>Data Type</th><th>Description</th><th>Mandatory</th></tr>
<tr><td>ClientReference</td><td>String</td><td>Client reference that was set in the request.</td><td>Yes</td></tr>
<tr><td>TransactionId</td><td>Integer</td><td>Unique transaction ID.</td><td>Yes</td></tr>
</table>


## Reverse payment

Use the Reverse payment method to cancel a payment already made. The transaction as a whole is then credited the mobile phone subscription. It is not possible to reverse only part of a payment.


### Request types
<table>
<tr><th>Name</th><th>Data Type</th><th>Description</th><th>Mandatory</th></tr>
<tr><td>ServiceCredentials</td><td>ServiceCredentials</td><td>See <a href="methods.md#servicecredentials">here</a> for details</td><td>Yes</td></tr>
<tr><td>ReversePaymentDetails</td><td>ReversePaymentDetails</td><td>Contains details about the transaction.</td><td>Yes</td></tr>
</table>

#### ReversePaymentDetails
<table>
<tr><th>Name</th><th>Data Type</th><th>Description</th><th>Mandatory</th></tr>
<tr><td>TransactionId</td><td>Integer</td><td>The unique transaction ID which was received in the payment response.</td><td>Yes</td></tr>
</table>


### Response types

#### ReversePaymentResponse
<table>
<tr><th>Name</th><th>Data Type</th><th>Description</th><th>Mandatory</th></tr>
<tr><td>TransactionId</td><td>Integer</td><td>The unique transaction ID which was received in the payment response.</td><td>Yes</td></tr>
</table>


## Get payment status

Use this method to get the payment status of a specific transaction. It can be useful in cases where something goes wrong in the payment or reverse payment flow and your client is unsure of the outcome of the operation.  


### Request types

#### PaymentStatusRequest
<table>
<tr><th>Name</th><th>Data Type</th><th>Description</th><th>Mandatory</th></tr>
<tr><td>ServiceCredentials</td><td>ServiceCredentials</td><td>See <a href="methods.md#servicecredentials">here</a> for details</td><td>Yes</td></tr>
<tr><td>ClientReference</td><td>String</td><td>The unique client reference which was specified in the pay request.</td><td>Yes</td></tr>
</table>

### Response types

#### PaymentStatusResponse
<table>
<tr><th>Name</th><th>Data Type</th><th>Description</th><th>Mandatory</th></tr>
<tr><td>ClientReference</td><td>String</td><td>The unique client reference which was specified in the payment status request.</td><td>Yes</td></tr>
<tr><td>PaymentStatus</td><td>Enum</td><td>Indicates the status of the payment.<br><br>
0 = Not paid <br>
1 = Paid <br>
2 = Reversed
</td><td>Yes</td></tr>
<tr><td>TransactionId</td><td>Integer</td><td>The unique transaction ID which was received in the payment response.</td><td>No</td></tr>
</table>


## Send verification code

Some of our services requires that the mobile phone number to be charged is verified. This is to ensure that end-user do not enter a random mobile phone number to be charged. To verify a mobile phone number, use the "Send verification code" method which will generate a pin code and send it by sms to the specified mobile phone number. Your client must then provide the end user with an interface where the pin code can be entered and verified. Even though it is optional to set the expiration time when using this functionality, keep in mind that people change phone numbers every now and then, so we advise to set the expiration time to 1 year ahead in time.


### Request types
<table>
<tr><th>Name</th><th>Data Type</th><th>Description</th><th>Mandatory</th></tr>
<tr><td>ServiceCredentials</td><td>ServiceCredentials</td><td>See <a href="methods.md#servicecredentials">here</a> for details</td><td>Yes</td></tr>
<tr><td>Msisdn</td><td>String</td><td>The mobile subscription number to be verified.</td><td>Yes</td></tr>
<tr><td>ExpiryTime</td><td>DateTime</td><td>The expiration time for a verification. If this field is not used, the verification lasts forever.</td><td>No</td></tr>
<tr><td>Template</td><td>String</td><td>The template that should be used in the SMS, use {0} as the placeholder for the code. A default template will be used if this parameter isn’t specified.<br><br>
Example: Your verification code is {0}. Have a nice day!</td><td>No</td></tr>
</table>

### Response types
N/A.

## Verify verification code

Use this method to verify that the pin code entered by the end-user matches the pin code generated by the system. 


### Request types
<table>
<tr><th>Name</th><th>Data Type</th><th>Description</th><th>Mandatory</th></tr>
<tr><td>ServiceCredentials</td><td>ServiceCredentials</td><td>See <a href="methods.md#servicecredentials">here</a> for details</td><td>Yes</td></tr>
<tr><td>Msisdn</td><td>String</td><td>The mobile subscription number associated with the verification code.</td><td>Yes</td></tr>
<tr><td>VerificationCode</td><td>String</td><td>The code to be verified.</td><td>Yes</td></tr>
</table>

### Response types
N/A.


## PreAuthorize

PreAuthorized payments are recurring payments where the End User have confirmed that the payment transactions (including premium SMS transactions) for a merchant can be executed without End User confirmation on each transaction.

This method returns a token to be used in subsequent Pay requests or Premium SMS requests. The token is issued by Strex and does not expire. However it can be manually invalidated by Strex in some situations if the Code of Conduct is violated.

Please note that the End User needs to be a user on the Strex platform for this to be successful.

NB This method is only available on the REST interface and it allows you to use the serviceId, username and password from either the SMS platform or Direct Pay platform. See the <a href="rest-interface.md">REST interface</a> for details regarding authentication.


### URI variables
<table>
<tr><th>Name</th><th>Data Type</th><th>Description</th></tr>
<tr><td>serviceId</td><td>String</td><td>The mobile subscription to generate a token for.</td></tr>
<tr><td>platform</td><td>Enum</td><td>Which platform the serviceId belongs to<br>
Accepts either int or string values:
0 = directpay <br>
1 = sms<br></td></tr>
</table>


### Request types
<table>
<tr><th>Name</th><th>Data Type</th><th>Description</th><th>Mandatory</th></tr>
<tr><td>Msisdn</td><td>String</td><td>The mobile subscription to generate a token for.</td><td>Yes</td></tr>
<tr><td>TransactionId</td><td>String</td><td>Your identifier for the request - is returned in the response.</td><td>Yes</td></tr>
<tr><td>Platform</td><td>Enum</td><td>Which Puzzel platform to validate ServiceCredentials against<br><br>
0 = DirectPay <br>
1 = Sms</td><td>Yes</td></tr>
<tr><td>SecurityLevel</td><td>Enum</td><td>Which security level the authorization request should use.<br><br>
1 = None <br>
2 = Confirmation<br>
3 = Pin (not in use)</td><td>Yes</td></tr>
<tr><td>ConfirmationChannel</td><td>Enum</td><td>If security level is set to 'Confirmation', you can specify which channel the confirmation message should use.<br><br>
1 = USSD <br>
2 = SMS</td><td>No</td></tr>
<tr><td>ShortNumber</td><td>String</td><td>If ConfirmationChannel is set to 'SMS', you can specify the originator prefix of the sms message here. Default is '2222' from Strex. Note that Strex adds a 10 digit suffix, so you should only use 4 digit short codes here.</td><td>No</td></tr>
<tr><td>MessagePrefix</td><td>String</td><td>If ConfirmationChannel is set to 'SMS', you can specify a custom text that will be preceding the standard text provided by Strex.</td><td>No</td></tr>
<tr><td>MessageSuffix</td><td>String</td><td>If ConfirmationChannel is set to 'SMS', you can specify a custom text that will be proceeding the standard text provided by Strex.</td><td>No</td></tr>
<tr><td>TokenDescription</td><td>String</td><td>Information what the token is used for. This description is shown on Strex "Min side" and should describe the subscription service.</td><td>No</td></tr>
<tr><td>Age</td><td>String</td><td>The minimum age of the End User required for this request.
If not set or present in the request, there is no age limit.</td><td>No</td></tr>
</table>

### Response types
<table>
<tr><th>Name</th><th>Data Type</th><th>Description</th></tr>
<tr><td>ExternalTransactionReference</td><td>String</td><td>A unique transaction reference from Strex.</td></tr>
<tr><td>Token</td><td>String</td><td>The token to be used with subscription sell requests.</td></tr>
<tr><td>TransactionId</td><td>String</td><td>Your inputted identifier for the request.</td></tr>
</table>


## Get user information
These methods returns various information from Strex regarding the End user and the subscription. 

**Note** these methods are only available on the REST endpoint.

### Get user information v1
Returns information about an end user's registration status on the Strex Payment Service.

#### Request
HTTP GET {host}/RestV1.svc/service/{serviceId}/msisdn/{msisdn}

#### Response object
<table>
<tr><th>Name</th><th>Data Type</th><th>Description</th></tr>
<tr><td>Msisdn</td><td>String</td><td>User's msisdn</td></tr>
<tr><td>Validity</td><td>Enum</td><td>
0 = Not registered<br>
1 = Registered, but not valid for licensed purchase<br>
2 = Valid for licensed purchase<br>
3 = MNO billing barred<br>
4 = Verified user with BankID and valid for licensed purchase<br>
</td></tr>
<tr><td>TerminalType</td><td>String</td><td>Handset model.</td></tr>
</table>

### Get user information v2
Runs an extensive "sell validation check" and provides an **indicative** response on whether the subscriber can be charged with the given input parameters.

#### Request
HTTP GET {host}/RestV1.svc/service/{serviceId}/msisdn/{msisdn}/userinfoV2

Query parameters
<table>
<tr><th>Name</th><th>Data Type</th><th>Description</th></tr>
<tr>
  <td>amount</td>
  <td>String</td>
  <td>Amount you plan to charge the subscriber. Given in Norwegian øre (ie 2500 = 25 NOK)
  </td>
</tr>
<tr><td>serviceCode</td><td>String</td><td>Strex service code you plan to use when charging the subscriber.</td></tr>
<tr><td>age</td><td>Integer</td><td>Optional. If provided, age of the subscriber is checked to be above the input age.</td></tr>
</table>

#### Response
Returns HTTP 200 if okay with an empty body.

### Get user information v3
Retrieves the current Strex transaction limits for an end user. The response is **indicative**. Only Strex limits is returned, MNO status is not checked (e.g. prepaid balance, payment card balance, cpa blocks, etc).

#### Request
HTTP GET {host}/RestV1.svc/service/{serviceId}/msisdn/{msisdn}/userinfoV3

#### Response object
<table>
<tr><th>Name</th><th>Data Type</th><th>Description</th></tr>
<tr><td>PreferredSourceOfFunds</td><td>Enum</td><td>
-1 = Unknown<br>
1 = MNO<br>
2 = Not in use<br>
3 = Payment card<br>
</td></tr>
<tr><td>SubscriptionType</td><td>Enum</td><td>
-1 = Unknown<br>
0 = Postpaid<br>
1 = Prepaid<br>
</td></tr>
<tr><td>MaxAmountPrTransaction</td><td>Integer</td><td>Maximum amount (in øre) which can be charged in a single sell transaction.</td></tr>
<tr><td>RemainingMonthlyAmount</td><td>Integer</td><td>Remaining monthly amount (in øre) which can be charge for the subscriber before exceeding the monthly limit.<br><br>
0 = limit already reached<br>
-1 = no limit applicable, subscriber valid for purchase above defined monthly limit.</td></tr>
<tr><td>RemainingYearlyAmount</td><td>Integer</td><td>Remaining yearly amount (in øre) which can be charge for the subscriber before exceeding the yearly limit.<br><br>
0 = limit already reached<br>
-1 = no limit applicable, subscriber valid for purchase above defined yearly limit.<br></td></tr>
</table>

### Get user information summary
Combines all the 3 "get user information" checks into one request and returns with 3 responses. 

It is also possible to run a real Sell request of 2 NOK which automatically will be reversed through this request. This feature must be enabled by Puzzel personnel on the service. Note that such transactions will be visible on the end users invoice.

#### Request
HTTP GET {host}/RestV1.svc/service/{serviceId}/msisdn/{msisdn}/userinfoSummary

Query parameters
<table>
<tr><th>Name</th><th>Data Type</th><th>Description</th></tr>
<tr><td>amount</td><td>String</td><td>Amount you plan to charge the subscriber. Given in Norwegian øre (ie 2500 = 25 NOK)</td></tr>
<tr><td>serviceCode</td><td>String</td><td>Strex service code you plan to use when charging the subscriber.</td></tr>
<tr><td>Age</td><td>Integer</td><td>Optional. If provided, age of the subscriber is checked to be above the input age.</td></tr>
</table>

#### Response

HTTP status code 200 indicates that successfull requests have been made and you will get a response object as defined below.
All other HTTP status codes indicates error, and HTTP status code 424 will return a DirectPaymentFault object.

<table>
<tr><th>Name</th><th>Data Type</th><th>Description</th></tr>
<tr><td>ResponseV1</td><td>Complex type</td><td>Returns the response object as defined in "Get user information" method</td></tr>
<tr><td>ResponseV1Fault</td><td>DirectPaymentFault</td><td>Returns any error codes from Strex on the "Get user information" request.</td></tr>
<tr><td>ResponseV2</td><td>Enum</td><td>An empty object (meaning success)</td></tr>
<tr><td>ResponseV2Fault</td><td>DirectPaymentFault</td><td>Returns any error codes from Strex on the "Get user information v2" request.</td></tr>
<tr><td>ResponseV3</td><td>Enum</td><td>Returns the response object as defined in "Get user information" method</td></tr>
<tr><td>ResponseV3Fault</td><td>DirectPaymentFault</td><td>Returns any error codes from Strex on the "Get user information v3" request.</td></tr>
</table>


## DirectPaymentFault

<table>
<tr><th>Name</th><th>Data Type</th><th>Description</th><th>Mandatory</th></tr>
<tr><td>Code</td><td>Integer</td><td>Error code, please refer to the <a href="error-codes.md">list of error codes</a> for more details.</td><td>Yes</td></tr>
<tr><td>Description</td><td>String</td><td>Contains details about the error that occurred.</td><td>Yes</td></tr>
</table>