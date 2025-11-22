
# Inventora API Documentation

## Authentication

All API requests require authentication using an API key. To obtain an API key, please contact us at hello@inventora.com.

Include your API key in requests using one of these methods:

**Header (recommended):**
```
x-inventora-api-key: your-api-key
```

**Query parameter:**
```
?x-inventora-api-key=your-api-key
```

## Base URL

```
https://app.inventora.com/api
```

## Endpoints

### Get Products

```
GET /products
```

Returns a paginated list of products.

**Query Parameters:**
- `limit` (optional): Number of results to return (default: 100, max: 1000)
- `offset` (optional): Number of results to skip (default: 0)

**Response:**
```json
{
  "data": [
    {
      "id": "string",
      "name": "string",
      "sku": "string",
      "stockLevel": 0,
      "unitType": "string",
      "unitPrice": 0,
      "minLevel": 0,
      "notes": "string",
      "retailPrice": 0,
      "wholesalePrice": 0,
      "ean": "string",
      "minimumQuantity": 0,
      "reorderURLs": ["string"],
      "locationStockLevels": {
        "Location Name": 0
      },
      "productionType": "infinite|single|batch|supplied|bundle",
      "materials": [
        {
          "id": "string",
          "name": "string",
          "sku": "string",
          "unitType": "string",
          "unitPrice": 0,
          "stockLevel": 0,
          "minLevel": 0,
          "quantityUsed": 0
        }
      ]
    }
  ],
  "pagination": {
    "totalCount": 0,
    "offset": 0,
    "limit": 100
  }
}
```

### Get Product

```
GET /products/:id
```

Returns a single product by ID.

**Response:**
```json
{
  "id": "string",
  "name": "string",
  "sku": "string",
  "stockLevel": 0,
  "unitType": "string",
  "unitPrice": 0,
  "minLevel": 0,
  "notes": "string",
  "retailPrice": 0,
  "wholesalePrice": 0,
  "ean": "string",
  "minimumQuantity": 0,
  "reorderURLs": ["string"],
  "locationStockLevels": {
    "Location Name": 0
  },
  "productionType": "infinite|single|batch|supplied|bundle",
  "materials": [
    {
      "id": "string",
      "name": "string",
      "sku": "string",
      "unitType": "string",
      "unitPrice": 0,
      "stockLevel": 0,
      "minLevel": 0,
      "quantityUsed": 0
    }
  ]
}
```

### Update Product

```
PATCH /products/:id
```

Updates a product's details.

**Request Body:**
```json
{
  "name": "string",
  "sku": "string",
  "unitType": "string",
  "unitPrice": 0,
  "minLevel": 0,
  "notes": "string",
  "retailPrice": 0,
  "wholesalePrice": 0,
  "ean": "string",
  "minimumQuantity": 0,
  "reorderURLs": ["string"]
}
```

**Note:** `unitPrice` can only be updated if the resource has no stock blocks tied to non-initial logs.

**Response:** Returns the updated product (same format as Get Product).

### Update Product Stock Level

```
POST /products/:id/stock-level-updates
```

Updates the stock level for a product at a specific location.

**Request Body:**
```json
{
  "quantity": 0,
  "locationName": "string",
  "updateType": "audit|restock|loss|custom",
  "notes": "string",
  "customStatusId": 0
}
```

**Fields:**
- `quantity` (required): The quantity to update
- `locationName` (required): Name of the location
- `updateType` (required): Type of update
  - `audit`: Sets absolute stock level
  - `restock`: Adds to stock level
  - `loss`: Removes from stock level
  - `custom`: Uses a custom status (requires `customStatusId`)
- `notes` (optional): Notes about the update
- `customStatusId` (required if `updateType` is `custom`): ID of the custom status

**Response:** Returns the updated product (same format as Get Product).

### Get Materials

```
GET /materials
```

Returns a paginated list of materials.

**Query Parameters:**
- `limit` (optional): Number of results to return (default: 100, max: 1000)
- `offset` (optional): Number of results to skip (default: 0)

**Response:** Same format as Get Products.

### Get Material

```
GET /materials/:id
```

Returns a single material by ID.

**Response:** Same format as Get Product.

### Update Material

```
PATCH /materials/:id
```

Updates a material's details.

**Request Body:** Same format as Update Product.

**Response:** Returns the updated material (same format as Get Product).

### Update Material Stock Level

```
POST /materials/:id/stock-level-updates
```

Updates the stock level for a material at a specific location.

**Request Body:** Same format as Update Product Stock Level.

**Response:** Returns the updated material (same format as Get Product).

### Get Locations

```
GET /locations
```

Returns a paginated list of locations.

**Query Parameters:**
- `limit` (optional): Number of results to return (default: 100, max: 1000)
- `offset` (optional): Number of results to skip (default: 0)

**Response:**
```json
{
  "data": [
    {
      "id": "string",
      "name": "string",
      "isDefault": false,
      "hideMaterials": false,
      "address1": "string",
      "address2": "string",
      "city": "string",
      "state": "string",
      "zip": "string",
      "country": "string"
    }
  ],
  "pagination": {
    "totalCount": 0,
    "offset": 0,
    "limit": 100
  }
}
```

## Types

### unitType

Valid unit types include:

**Count:**
- `pieces`
- `bundles`

**Weight:**
- `weight.lbs`
- `weight.oz`
- `weight.kg`
- `weight.grams`
- `weight.mg`
- `weight.ct`
- `weight.gr`

**Length:**
- `length.ft`
- `length.yd`
- `length.in`
- `length.m`
- `length.cm`
- `length.mm`

**Area:**
- `area.sqft`
- `area.sqin`
- `area.sqm`
- `area.sqcm`

**Volume:**
- `volume.oz`
- `volume.pt`
- `volume.qt`
- `volume.ga`
- `volume.ml`
- `volume.liters`
- `volume.bdft`
- `volume.cuin`
- `volume.cuft`
- `volume.cuyd`
- `volume.cm3`
- `volume.m3`
- `volume.tsp`
- `volume.tbsp`
- `volume.cup`

**Time:**
- `time.seconds`
- `time.minutes`
- `time.hours`
- `time.days`

