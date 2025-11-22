
# Inventora API Documentation

## Authentication

All API endpoints require authentication using an API key. 

To obtain an API key, please email hello@inventora.com.

### Passing the API Key

The API key can be passed in one of two ways:

1. **Header (Recommended)**: Include the key in the request header
   ```
   x-inventora-api-key: your-api-key-here
   ```

2. **Query Parameter**: Append the key to the URL
   ```
   ?x-inventora-api-key=your-api-key-here
   ```

## Base URL

All endpoints are relative to your API base URL.

## Endpoints

### Get Products

Retrieve a paginated list of products.

**Endpoint:** `GET /products`

**Query Parameters:**
- `limit` (optional): Number of results to return. Maximum 1000, default 100.
- `offset` (optional): Number of results to skip. Default 0.

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
      "productionType": "infinite|single|produced|supplied|bundle",
      "locationStockLevels": {
        "Location Name": "number"
      },
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
    "totalCount": 0,
    "offset": 0,
    "limit": 100
  }
}
```

### Get Materials

Retrieve a paginated list of materials (supplies).

**Endpoint:** `GET /materials`

**Query Parameters:**
- `limit` (optional): Number of results to return. Maximum 1000, default 100.
- `offset` (optional): Number of results to skip. Default 0.

**Response:**

Same structure as Get Products endpoint.

### Get Locations

Retrieve a paginated list of locations.

**Endpoint:** `GET /locations`

**Query Parameters:**
- `limit` (optional): Number of results to return. Maximum 1000, default 100.
- `offset` (optional): Number of results to skip. Default 0.

**Response:**
```json
{
  "data": [
    {
      "id": "string",
      "OrganizationId": "string",
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

### Update Product

Update a product by ID.

**Endpoint:** `PATCH /products/:id`

**URL Parameters:**
- `id`: The product ID

**Request Body:**

All fields are optional. Only include fields you want to update.

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

**Notes:**
- `unitPrice` cannot be updated if the product has stock blocks tied to non-initial logs
- When updating `productionType`, use "produced" for batch production

**Response:**

Returns the updated product with the same structure as a single item from the Get Products endpoint.

**Error Responses:**
- `400`: Invalid request or unitPrice cannot be updated
- `404`: Product not found

### Update Material

Update a material (supply) by ID.

**Endpoint:** `PATCH /materials/:id`

**URL Parameters:**
- `id`: The material ID

**Request Body:**

Same structure as Update Product endpoint.

**Response:**

Returns the updated material with the same structure as a single item from the Get Materials endpoint.

**Error Responses:**
- `400`: Invalid request or unitPrice cannot be updated
- `404`: Material not found

## Error Responses

All endpoints may return the following error responses:

**401 Unauthorized:**
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
```json
{
  "error": "Organization not found"
}
```

## Unit Types

The following unit types are supported:

- Pieces: `pieces`, `bundles`
- Weight: `weight.lbs`, `weight.oz`, `weight.kg`, `weight.grams`, `weight.mg`, `weight.ct`, `weight.gr`
- Length: `length.ft`, `length.yd`, `length.in`, `length.m`, `length.cm`, `length.mm`
- Area: `area.sqft`, `area.sqin`, `area.sqm`, `area.sqcm`
- Volume: `volume.oz`, `volume.pt`, `volume.qt`, `volume.ga`, `volume.ml`, `volume.liters`, `volume.bdft`, `volume.cuin`, `volume.cuft`, `volume.cuyd`, `volume.cm3`, `volume.m3`, `volume.tsp`, `volume.tbsp`, `volume.cup`
- Time: `time.seconds`, `time.minutes`, `time.hours`, `time.days`

## Production Types

- `infinite`: Infinite supply
- `single`: Single item production
- `produced`: Batch production
- `supplied`: Supplied from materials
- `bundle`: Bundle of products

