## Products

### GET

Returns a paged list of products, including all product details.

#### Example

Request:

```
GET /v1/products.json
```

Response:

```json
  {
    "products": [
      {
        "id": 104,
        "name": "iPhone 6",
        "categories": [
          "cellphones",
          "electronics",
          "apple"
        ],
        "price": "999.99",
        "description": "A thing of beauty."
      },
      {
        "id": 45,
        "name": "Nexus 6p",
        "categories": [
          "cellphones",
          "electronics",
          "google"
        ],
        "price": "333.33",
        "description": "Great phone, better price."
      }
    ]
  }
```

#### Parameters
Required are **bold**

| Name     | Default | Description                                                          |
| ----     | ------- | -------------------------------------------------------------------- |
| page     | 1       | The page of products to view.                                        |
| per_page | 20      | The number of products to display per page.                          |
| q        |         | A search string used for matching on product names and descriptions. |

The defaults are used when no value is provided or if it is invalid (non-numeric, etc).
