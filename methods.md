# API Methods and Types

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
<tr><td>ServiceId</td><td>Integer</td><td>Service ID, provided by Intelecom service desk.</td><td>Yes</td></tr>
<tr><td>Username</td><td>String</td><td>Username, provided by Intelecom service desk.</td><td>Yes</td></tr>
<tr><td>Password</td><td>String</td><td>Password, provided by Intelecom service desk.</td><td>Yes</td></tr>
</table>


#### PaymentDetails
<table>
<tr><th>Name</th><th>Data Type</th><th>Description</th><th>Mandatory</th></tr>
<tr><td>Msisdn</td><td>String</td><td>The MSISDN that should be charged. The format should follow the ITU-T E.164 standard with a + prefix.</td><td>Yes</td></tr>
<tr><td>Price</td><td>Integer</td><td>The amount that should be debited, in lowest monetary unit. Example: 100 (1,- NOK).</td><td>Yes</td></tr>
<tr><td>ServiceCode</td><td>String</td><td>Service code identifying the type of transaction. See <a href="https://github.com/Intelecom/direct-payment/blob/master/service-codes.md">here</a> for a list of available service codes</td><td>Yes</td></tr>
<tr><td>ClientReference</td><td>String</td><td>Client reference for the transaction, must be unique.</td><td>Yes</td></tr>
<tr><td>EndUserInvoiceText</td><td>String</td><td>Text that will be displayed on the end-user invoice.</td><td>No</td></tr>
<tr><td>Age</td><td>Integer</td><td>The minimum age of the subscriber required for the purchase. If set, the valid values are 16 and 18.</td><td>No</td></tr>
<tr><td>Differentiator</td><td>String</td><td>Arbitrary string set by the client to enable grouping of messages in certain statistics reports.</td><td>No</td></tr>
<tr><td>InvoiceNode</td><td>String</td><td>Arbitrary string set by the client to enable grouping of messages on the service invoice.</td><td>No</td></tr>
<tr><td>BusinessModel</td><td>String</td><td>Business model, a list containing the available business models is available <a href="https://github.com/Intelecom/direct-payment/blob/master/business-models.md">here</a>.</td><td>No</td></tr>
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
<tr><td>ServiceCredentials</td><td>ServiceCredentials</td><td>See <a href="https://github.com/Intelecom/direct-payment/blob/master/methods.md#servicecredentials">here</a> for details</td><td>Yes</td></tr>
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
<tr><td>ServiceCredentials</td><td>ServiceCredentials</td><td>See <a href="https://github.com/Intelecom/direct-payment/blob/master/methods.md#servicecredentials">here</a> for details</td><td>Yes</td></tr>
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
</table>


## Send verification code

Some of our services requires that the mobile phone number to be charged is verified. This is to ensure that end-user do not enter a random mobile phone number to be charged. To verify a mobile phone number, use the "Send verification code" method which will generate a pin code and send it by sms to the specified mobile phone number. Your client must then provide the end user with an interface where the pin code can be entered and verified. Even though it is optional to set the expiration time when using this functionality, keep in mind that people change phone numbers every now and then, so we advise to set the expiration time to 1 year ahead in time.


### Request types
<table>
<tr><th>Name</th><th>Data Type</th><th>Description</th><th>Mandatory</th></tr>
<tr><td>ServiceCredentials</td><td>ServiceCredentials</td><td>See <a href="https://github.com/Intelecom/direct-payment/blob/master/methods.md#servicecredentials">here</a> for details</td><td>Yes</td></tr>
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
<tr><td>ServiceCredentials</td><td>ServiceCredentials</td><td>See <a href="https://github.com/Intelecom/direct-payment/blob/master/methods.md#servicecredentials">here</a> for details</td><td>Yes</td></tr>
<tr><td>Msisdn</td><td>String</td><td>The mobile subscription number associated with the verification code.</td><td>Yes</td></tr>
<tr><td>VerificationCode</td><td>string</td><td>The code to be verified.</td><td>Yes</td></tr>
</table>

### Response types
N/A.


## DirectPaymentFault

<table>
<tr><th>Name</th><th>Data Type</th><th>Description</th><th>Mandatory</th></tr>
<tr><td>Code</td><td>Integer</td><td>Error code, please refer to the <a href="https://github.com/Intelecom/direct-payment/blob/master/error-codes.md">list of error codes</a> for more details.</td><td>Yes</td></tr>
<tr><td>Description</td><td>String</td><td>Contains details about the error that occurred.</td><td>Yes</td></tr>
</table>