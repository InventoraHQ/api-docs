
# Inventora API Documentation

## Authentication

All API requests require authentication using an API key. To obtain an API key, please contact hello@inventora.com.

Include your API key in requests using either:
- Header: `X-Inventora-API-Key: your_api_key`
- Query parameter: `?x-inventora-api-key=your_api_key`

## Base URL

All endpoints are relative to your API base URL.

## Endpoints

### Products

#### List Products

```
GET /products
```

Returns a paginated list of products.

**Query Parameters:**
- `limit` (optional): Number of results to return (max 1000, default 100)
- `offset` (optional): Number of results to skip (default 0)

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
        "locationName": "number"
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
    "totalCount": "number",
    "offset": "number",
    "limit": "number"
  }
}
```

#### Get Product

```
GET /products/:id
```

Returns a single product by ID.

**Response:**
Same as individual product object in List Products response.

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

All fields are optional. Only included fields will be updated.

**Note:** `unitPrice` cannot be updated if the resource has stock blocks tied to non-initial logs.

**Response:**
Returns the updated product object.

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

**Fields:**
- `quantity` (required): The quantity to set or adjust
- `locationName` (required): Name of the location
- `updateType` (required): Type of update
  - `audit`: Sets stock to absolute value
  - `restock`: Adds to current stock
  - `loss`: Removes from current stock
  - `custom`: Custom status (requires `customStatusId`)
- `notes` (optional): Notes about the update
- `customStatusId` (required for `custom` type): ID of custom status

**Response:**
Returns the updated product object with new stock levels.

### Materials

#### List Materials

```
GET /materials
```

Returns a paginated list of materials. Parameters and response format are identical to List Products.

#### Get Material

```
GET /materials/:id
```

Returns a single material by ID. Response format is identical to Get Product.

#### Update Material

```
PATCH /materials/:id
```

Updates a material's details. Request body and response format are identical to Update Product.

#### Update Material Stock Level

```
POST /materials/:id/stock-level-updates
```

Updates the stock level for a material at a specific location. Request body and response format are identical to Update Product Stock Level.

### Locations

#### List Locations

```
GET /locations
```

Returns a paginated list of locations.

**Query Parameters:**
- `limit` (optional): Number of results to return (max 1000, default 100)
- `offset` (optional): Number of results to skip (default 0)

**Response:**
```json
{
  "data": [
    {
      "id": "string",
      "OrganizationId": "string",
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
    "totalCount": "number",
    "offset": "number",
    "limit": "number"
  }
}
```

## Types

### unitType

Valid unit types:
- `pieces`
- `bundles`
- Weight: `weight.lbs`, `weight.oz`, `weight.kg`, `weight.grams`, `weight.mg`, `weight.ct`, `weight.gr`
- Length: `length.ft`, `length.yd`, `length.in`, `length.m`, `length.cm`, `length.mm`
- Area: `area.sqft`, `area.sqin`, `area.sqm`, `area.sqcm`
- Volume: `volume.oz`, `volume.pt`, `volume.qt`, `volume.ga`, `volume.ml`, `volume.liters`, `volume.bdft`, `volume.cuin`, `volume.cuft`, `volume.cuyd`, `volume.cm3`, `volume.m3`, `volume.tsp`, `volume.tbsp`, `volume.cup`
- Time: `time.seconds`, `time.minutes`, `time.hours`, `time.days`

## Error Responses

Error responses will include an `error` field with a description:

```json
{
  "error": "Error message description"
}
```

Common HTTP status codes:
- `400`: Bad Request - Invalid parameters or request body
- `401`: Unauthorized - Missing or invalid API key
- `404`: Not Found - Resource not found

