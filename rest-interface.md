# REST Interface

**Important:** This interface uses Pascal casing and is case-sensitive.

### How to connect

The REST interface supports both JSON and XML over HTTP with the following base URI: [https://directpayment.intele.com/restV1.svc](https://directpayment.intele.com/restV1.svc)


XML schema definitions and example requests/responses may be found at: [https://directpayment.intele.com/restV1.svc/help](https://directpayment.intele.com/restV1.svc/help)


### Authentication

Clients must authenticate themselves using basic authentication. The username and password are extracted from the “Authorization” HTTP header.

**Note:** The serviceId will be extracted from the request URL.

Example with username “foo” and password “bar”:

*Authorization: Basic Zm9vOmJhcg==*


### Error handling

The REST interface will respond with varying HTTP status codes, depending on the error type. The HTTP body includes details containing application specific information, which is of type [DirectPaymentFault](methods.md#directpaymentfault).


### Error example

```HTTP/1.1 400 Bad Request```

```Content-Type: application/json; charset=utf-8```

```Content-Length: 38```

```{"Code":2,"Description":"Invalid age"}```


### Content types

The REST endpoint supports both JSON and XML. To specify the content type of the request, you must specify the “*Content-Type*” header and to specify the content type of the response, you must specify the “*Accept*” header. 

Valid values are “*application/json*” and “*application/xml*”.


### Encoding

The content encoding should be UTF-8.