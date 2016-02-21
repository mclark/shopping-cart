## Orders

### POST

Creates a new order based on the contents of the cart.

#### Example

Request:

```
POST /v1/orders.json
```

Response:

```json
  {
    "order": {
      "id": 1005,
      "products": [
        {
          "product_id": 104,
          "quantity": 2
        },
        {
          "product_id": 33,
          "quantity": 1
        }
      ]
    }
  }
```

#### Parameters

No parameters are supported for this endpoint.
