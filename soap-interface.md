# Web Service (SOAP) Interface

### How to connect

The web service interface is defined by the WSDL and may be retrieved from [https://directpayment.intele.com/soapV1.svc?singleWsdl](https://directpayment.intele.com/soapV1.svc?singleWsdl)


### Error handling

The web service will respond with a SOAP fault when an API error occurs. Where applicable the SOAP fault will include details containing application specific information, which is of type [DirectPaymentFault](methods.md#directpaymentfault).

### Error example

 
	<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
	  <s:Body>
        <s:Fault>
         <faultcode xmlns:a="http://schemas.microsoft.com/2009/WebFault">a:BadRequest</faultcode>
         <faultstring xml:lang="nb-NO">Bad Request</faultstring>
         <detail>
            <DirectPaymentFault xmlns="http://directpay.intele.com/v1/faults" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
               <Code>2</Code>
               <Description>Invalid age</Description>
            </DirectPaymentFault>
         </detail>
        </s:Fault>
	  </s:Body>
	</s:Envelope>   
