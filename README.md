
# Inventora API Documentation

## Authentication

All API requests require authentication using an API key. To obtain an API key, please contact hello@inventora.com.

Include your API key in one of two ways:

1. As a header: `x-inventora-api-key: YOUR_API_KEY`
2. As a query parameter: `?x-inventora-api-key=YOUR_API_KEY`

## Base URL

All endpoints are relative to your API base URL.

## Common Parameters

### Pagination

List endpoints support pagination with the following query parameters:

- `limit` (optional): Number of results to return (default: 100, max: 1000)
- `offset` (optional): Number of results to skip (default: 0)

### Response Format

Paginated responses include:

```json
{
  "data": [...],
  "pagination": {
    "totalCount": 150,
    "offset": 0,
    "limit": 100
  }
}
```

## Endpoints

### Products

#### List Products

```
GET /products
```

Returns a paginated list of products.

**Query Parameters:**
- `limit` (optional): Number of results (default: 100, max: 1000)
- `offset` (optional): Results to skip (default: 0)

**Response:**
```json
{
  "data": [
    {
      "id": "string",
      "name": "string",
      "sku": "string",
      "stockLevel": "number",
      "unitType": "string",
      "unitPrice": "number",
      "minLevel": "number",
      "notes": "string",
      "retailPrice": "number",
      "wholesalePrice": "number",
      "ean": "string",
      "minimumQuantity": "number",
      "reorderURLs": ["string"],
      "locationStockLevels": {
        "Location Name": "number"
      },
      "productionType": "infinite|single|produced|supplied|bundle",
      "materials": [
        {
          "id": "string",
          "name": "string",
          "sku": "string",
          "unitType": "string",
          "unitPrice": "number",
          "stockLevel": "number",
          "minLevel": "number",
          "quantityUsed": "number"
        }
      ]
    }
  ],
  "pagination": {
    "totalCount": 150,
    "offset": 0,
    "limit": 100
  }
}
```

#### Get Product

```
GET /products/:id
```

Returns a single product by ID.

**Response:** Same as a single product object from the list endpoint.

#### Update Product

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
  "unitPrice": "number",
  "minLevel": "number",
  "productionType": "infinite|single|produced|supplied|bundle",
  "notes": "string",
  "retailPrice": "number",
  "wholesalePrice": "number",
  "ean": "string",
  "minimumQuantity": "number",
  "reorderURLs": ["string"]
}
```

All fields are optional. Only include fields you want to update.

**Note:** `unitPrice` cannot be updated if the product has stock blocks tied to non-initial logs.

**Response:** Updated product object.

#### Update Product Stock Level

```
POST /products/:id/stock-level-updates
```

Updates the stock level for a product at a specific location.

**Request Body:**
```json
{
  "quantity": "number",
  "locationName": "string",
  "updateType": "audit|restock|loss|custom",
  "notes": "string",
  "customStatusId": "number"
}
```

**Required fields:**
- `quantity`: The quantity to set (for audit) or adjust (for other types)
- `locationName`: Name of the location
- `updateType`: Type of stock update
  - `audit`: Sets stock to the exact quantity specified
  - `restock`: Adds to current stock level
  - `loss`: Subtracts from current stock level
  - `custom`: Uses a custom status (requires `customStatusId`)

**Optional fields:**
- `notes`: Additional notes about the update
- `customStatusId`: Required when `updateType` is "custom"

**Response:** Updated product object with new stock levels.

### Materials

#### List Materials

```
GET /materials
```

Returns a paginated list of materials (supplies).

**Query Parameters:**
- `limit` (optional): Number of results (default: 100, max: 1000)
- `offset` (optional): Results to skip (default: 0)

**Response:** Same format as products list endpoint.

#### Get Material

```
GET /materials/:id
```

Returns a single material by ID.

**Response:** Same as a single material object from the list endpoint.

#### Update Material

```
PATCH /materials/:id
```

Updates a material's details.

**Request Body:** Same as update product endpoint.

**Response:** Updated material object.

#### Update Material Stock Level

```
POST /materials/:id/stock-level-updates
```

Updates the stock level for a material at a specific location.

**Request Body:** Same as update product stock level endpoint.

**Response:** Updated material object with new stock levels.

### Locations

#### List Locations

```
GET /locations
```

Returns a paginated list of locations.

**Query Parameters:**
- `limit` (optional): Number of results (default: 100, max: 1000)
- `offset` (optional): Results to skip (default: 0)

**Response:**
```json
{
  "data": [
    {
      "id": "string",
      "name": "string",
      "isDefault": "boolean",
      "hideMaterials": "boolean",
      "address1": "string",
      "address2": "string",
      "city": "string",
      "state": "string",
      "zip": "string",
      "country": "string"
    }
  ],
  "pagination": {
    "totalCount": 10,
    "offset": 0,
    "limit": 100
  }
}
```

## Error Responses

All endpoints may return the following error responses:

**401 Unauthorized**
```json
{
  "error": "API key missing"
}
```
```json
{
  "error": "Invalid API key"
}
```

**400 Bad Request**
```json
{
  "error": "Error message describing the issue"
}
```

**404 Not Found**
```json
{
  "error": "Resource not found"
}
```

## Field Definitions

### unitType

Possible values for the `unitType` field:

- `pieces`
- `bundles`
- **Weight:** `weight.lbs`, `weight.oz`, `weight.kg`, `weight.grams`, `weight.mg`, `weight.ct`, `weight.gr`
- **Length:** `length.ft`, `length.yd`, `length.in`, `length.m`, `length.cm`, `length.mm`
- **Area:** `area.sqft`, `area.sqin`, `area.sqm`, `area.sqcm`
- **Volume:** `volume.oz`, `volume.pt`, `volume.qt`, `volume.ga`, `volume.ml`, `volume.liters`, `volume.bdft`, `volume.cuin`, `volume.cuft`, `volume.cuyd`, `volume.cm3`, `volume.m3`, `volume.tsp`, `volume.tbsp`, `volume.cup`
- **Time:** `time.seconds`, `time.minutes`, `time.hours`, `time.days`

