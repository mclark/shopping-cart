## Cart

### GET

Displays the current contents of the shopping cart.

#### Example

Request:

```
GET /v1/cart.json
```

Response:

```json
  {
    "cart": {
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

### PATCH

Update the quantity of a product in the shopping cart. Setting quantity to 0 removes the product.
Response contains the updated cart resource.

#### Example

Request:

```
PATCH /v1/cart.json?product_id=45&quantity=3
```

Response (assuming contents from example above):

```json
  {
    "cart": {
      "products": [
        {
          "product_id": 104,
          "quantity": 2
        },
        {
          "product_id": 33,
          "quantity": 1
        },
        {
          "product_id": 45,
          "quantity": 3
        }
      ]
    }
  }
```

#### Parameters

Required are **bold**

| Name           | Default | Description                                    |
| ----           | ------- | ---------------------------------------------- |
| **product_id** |         | The id of the product to be added to the cart. |
| quantity       | 1       | The quantity of the product to add.            |

#### Errors

| Code | Description                        |
| ---- | ---------------------------------- |
| 1000 | The product could not be found.    |
| 1001 | A product must be specified.       |
