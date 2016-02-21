## Shopping Cart API

This document describes the API interface to the shopping cart feature as described [here](https://docs.google.com/document/d/1SEgRYzaLnuIJ669NCLmiY94iGfI2JHEP8Jsh1iLxViU/).

### Url Contexts

It is suggested the API be hosted on a dedicated subdomain to simplify traffic handling
and monitoring. All endpoints of the API are nested under an appropriate version context
such as `v1`. This will allow major breaking changes of the API to be implemented without
breaking existing entrypoints as they are used by existing clients.

All requests to the API are required to be secured using SSL. The API server will only
listen on the secured port, therefore all plaintext requests will not even connect.

An example url:

```
https://api.example.com/v1/products.json
```

### Authentication

Clients, or users, are assumed to be initially authenticated by creating a new account
on the shopping cart system. There are numerous ways of accomplishing this and the
specifics are currently out of scope for this document. Suggested methods include
single sign on using Facebook or Google or others, or just plain old email/password signup.

However the account is created, the end result is the generation of two secrets, a
*secret key* and a *secret id*. Both are randomly generated and given to the client
as a response to the initial signup process. Both are also stored on the server in a
secure database table.

When a request is made, the url and body are then used as the data to create a hashed
value which, when combined with the *secret id*, creates a *signature* for the request.
This provides assurance that the client is authentic and that the request was not tampered
with in transit.

The signature is then specified in the `Authorization` header of the request in the format.
In this example, we have used the SHA256 hash algorithm, but the server can choose to
support whatever algorithm is preferred.

```
Authorization: CART-HMAC-SHA256 Client=<SECRET_ID>,Signature=<SIGNATURE>
```

If the client were to reset their key (indirectly via password reset, or directly via
other means) a new key would be generated and returned. All other client instances using the
old key would be unauthenticated.

#### Implementation Considerations

##### Secret Keys

When the server receives a signed request, the account record is first looked up using the
*secret id*. Next, the *secret key* is used to hash the url and body of the request to ensure
the hashed value matches that provided by the client.

Storing all the keys in plaintext in the database is a security risk, so encrypting them
with a symmetric cipher and storing the key to that in another location would be a good
practice. Performance issues could be mitigated using memory caching. That's more an
implementation consideration.

#### Session authentication

While not RESTful, session based token authentication could augment the above security strategy.
Instead of using long lived secrets and ids, the client could `POST` to a sessions
resource endpoint, receive a time boxed key and id, and use them for all requests moving forward.
These would then function as above, the difference being that they will eventually time out
and become invalid. These could then be stored in a timed cache server side.

### Error Response Codes

This is a list of response codes expected to be handled by the application code itself. All HTTP
response codes are in scope however as higher layers on the stack may be able to return
meaningful response codes prior to the application's execution.

#### 400 Bad Request

This is returned when a client error has prevented the request from executing successfully. The body
of this request contains the details of the failure in JSON format.

```json
{
  "errors": [
    {
      "code": 1234,
      "message": "Quantity is required."
    }
  ]
}
```

The response is wrapped in an object hash to allow for future extensibility. The one
property, errors, contains a list of the errors to be returned. Each error contains
the following two fields:

* **code**: a unique positive integer designating the error
* **message**: a human readable message, intended for use by the application developer.

#### 401 Unauthorized

This error code is returned without body when the request fails authentication.

#### 404 Not Found

This will be returned with no body when the requested resource cannot be found.

#### 500 Server Error

This is generated when the server has an unexpected error preventing the request from being completed.
It is the responsibility of the server developers to resolve this by examining logs
and monitoring data.

### Supported Content Types

The response content type for this application is `application/json`. Other types can easily
be added in the future. Request bodies can be written in `application/json` or
`application/x-www-form-urlencoded`.

### Resources

* [Products](/docs/products.md)
* [Cart](/docs/cart.md)
* [Orders](/docs/orders.md)
