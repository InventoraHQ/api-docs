
# Inventora API Documentation

## Authentication

All API requests require authentication using an API key. You can obtain an API key by emailing hello@inventora.com.

Include your API key in one of two ways:

1. As a header: `X-Inventora-API-Key: your_api_key_here`
2. As a query parameter: `?x-inventora-api-key=your_api_key_here`

## Base URL

```
https://api.inventora.com/api
```

## Endpoints

### Get Products

```
GET /products
```

Returns a paginated list of products.

**Query Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| limit | number | 100 | Maximum number of results (max: 1000) |
| offset | number | 0 | Number of results to skip |

**Response:**

```json
{
  "data": [
    {
      "id": "prod_123",
      "name": "Handmade Candle",
      "sku": "CANDLE-001",
      "stockLevel": 45,
      "unitType": "pieces",
      "unitPrice": 5.50,
      "minLevel": 10,
      "notes": "Vanilla scented",
      "retailPrice": 15.00,
      "wholesalePrice": 10.00,
      "ean": "1234567890123",
      "minimumQuantity": 1,
      "reorderURLs": ["https://example.com/reorder"],
      "locationStockLevels": {
        "Main Warehouse": 30,
        "Retail Store": 15
      },
      "productionType": "produced",
      "materials": [
        {
          "id": "mat_456",
          "name": "Soy Wax",
          "sku": "WAX-SOY",
          "unitType": "weight.lbs",
          "unitPrice": 3.25,
          "stockLevel": 100,
          "minLevel": 20,
          "quantityUsed": 0.5
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

### Get Product

```
GET /products/:id
```

Returns a single product by ID.

**Response:**

```json
{
  "id": "prod_123",
  "name": "Handmade Candle",
  "sku": "CANDLE-001",
  "stockLevel": 45,
  "unitType": "pieces",
  "unitPrice": 5.50,
  "minLevel": 10,
  "notes": "Vanilla scented",
  "retailPrice": 15.00,
  "wholesalePrice": 10.00,
  "ean": "1234567890123",
  "minimumQuantity": 1,
  "reorderURLs": ["https://example.com/reorder"],
  "locationStockLevels": {
    "Main Warehouse": 30,
    "Retail Store": 15
  },
  "productionType": "produced",
  "materials": [
    {
      "id": "mat_456",
      "name": "Soy Wax",
      "sku": "WAX-SOY",
      "unitType": "weight.lbs",
      "unitPrice": 3.25,
      "stockLevel": 100,
      "minLevel": 20,
      "quantityUsed": 0.5
    }
  ]
}
```

### Get Materials

```
GET /materials
```

Returns a paginated list of materials.

**Query Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| limit | number | 100 | Maximum number of results (max: 1000) |
| offset | number | 0 | Number of results to skip |

**Response:**

```json
{
  "data": [
    {
      "id": "mat_456",
      "name": "Soy Wax",
      "sku": "WAX-SOY",
      "stockLevel": 100,
      "unitType": "weight.lbs",
      "unitPrice": 3.25,
      "minLevel": 20,
      "notes": "Food grade soy wax",
      "retailPrice": null,
      "wholesalePrice": null,
      "ean": "9876543210987",
      "minimumQuantity": 10,
      "reorderURLs": ["https://supplier.com/wax"],
      "locationStockLevels": {
        "Main Warehouse": 80,
        "Production Floor": 20
      },
      "productionType": "supplied",
      "materials": []
    }
  ],
  "pagination": {
    "totalCount": 75,
    "offset": 0,
    "limit": 100
  }
}
```

### Get Material

```
GET /materials/:id
```

Returns a single material by ID.

**Response:**

```json
{
  "id": "mat_456",
  "name": "Soy Wax",
  "sku": "WAX-SOY",
  "stockLevel": 100,
  "unitType": "weight.lbs",
  "unitPrice": 3.25,
  "minLevel": 20,
  "notes": "Food grade soy wax",
  "retailPrice": null,
  "wholesalePrice": null,
  "ean": "9876543210987",
  "minimumQuantity": 10,
  "reorderURLs": ["https://supplier.com/wax"],
  "locationStockLevels": {
    "Main Warehouse": 80,
    "Production Floor": 20
  },
  "productionType": "supplied",
  "materials": []
}
```

### Update Product

```
PATCH /products/:id
```

Updates a product.

**Request Body:**

```json
{
  "name": "Updated Product Name",
  "sku": "NEW-SKU",
  "unitType": "pieces",
  "unitPrice": 6.00,
  "minLevel": 15,
  "notes": "Updated notes",
  "retailPrice": 18.00,
  "wholesalePrice": 12.00,
  "ean": "1234567890124",
  "minimumQuantity": 2,
  "reorderURLs": ["https://example.com/reorder-new"]
}
```

All fields are optional. Only include fields you want to update.

**Note:** `unitPrice` can only be updated if the resource has no stock blocks tied to non-initial logs. The `unitType` field cannot be changed to or from `bundle`.

**Response:**

```json
{
  "id": "prod_123",
  "name": "Updated Product Name",
  "sku": "NEW-SKU",
  "stockLevel": 45,
  "unitType": "pieces",
  "unitPrice": 6.00,
  "minLevel": 15,
  "notes": "Updated notes",
  "retailPrice": 18.00,
  "wholesalePrice": 12.00,
  "ean": "1234567890124",
  "minimumQuantity": 2,
  "reorderURLs": ["https://example.com/reorder-new"],
  "locationStockLevels": {
    "Main Warehouse": 30,
    "Retail Store": 15
  },
  "productionType": "produced",
  "materials": [
    {
      "id": "mat_456",
      "name": "Soy Wax",
      "sku": "WAX-SOY",
      "unitType": "weight.lbs",
      "unitPrice": 3.25,
      "stockLevel": 100,
      "minLevel": 20,
      "quantityUsed": 0.5
    }
  ]
}
```

### Update Material

```
PATCH /materials/:id
```

Updates a material.

**Request Body:**

```json
{
  "name": "Updated Material Name",
  "sku": "NEW-MAT-SKU",
  "unitType": "weight.kg",
  "unitPrice": 4.00,
  "minLevel": 25,
  "notes": "Updated material notes",
  "retailPrice": null,
  "wholesalePrice": null,
  "ean": "9876543210988",
  "minimumQuantity": 15,
  "reorderURLs": ["https://supplier.com/wax-new"]
}
```

All fields are optional. Only include fields you want to update.

**Note:** `unitPrice` can only be updated if the resource has no stock blocks tied to non-initial logs. The `unitType` field cannot be changed to or from `bundle`.

**Response:**

```json
{
  "id": "mat_456",
  "name": "Updated Material Name",
  "sku": "NEW-MAT-SKU",
  "stockLevel": 100,
  "unitType": "weight.kg",
  "unitPrice": 4.00,
  "minLevel": 25,
  "notes": "Updated material notes",
  "retailPrice": null,
  "wholesalePrice": null,
  "ean": "9876543210988",
  "minimumQuantity": 15,
  "reorderURLs": ["https://supplier.com/wax-new"],
  "locationStockLevels": {
    "Main Warehouse": 80,
    "Production Floor": 20
  },
  "productionType": "supplied",
  "materials": []
}
```

### Update Product Stock Level

```
POST /products/:id/stock-level-updates
```

Updates the stock level of a product at a specific location.

**Request Body:**

```json
{
  "quantity": 10,
  "locationName": "Main Warehouse",
  "updateType": "audit",
  "notes": "Weekly inventory count",
  "customStatusId": null
}
```

**Fields:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| quantity | number | Yes | The quantity to set or adjust |
| locationName | string | Yes | Name of the location |
| updateType | string | Yes | One of: `audit`, `restock`, `loss`, `custom` |
| notes | string | No | Optional notes about the update |
| customStatusId | number | Conditional | Required when updateType is `custom` |

**Update Types:**

- `audit`: Sets the stock level to the exact quantity specified (absolute value)
- `restock`: Adds the quantity to current stock level (adjustment)
- `loss`: Subtracts the quantity from current stock level (adjustment)
- `custom`: Uses a custom status (requires `customStatusId`)

**Response:**

```json
{
  "id": "prod_123",
  "name": "Handmade Candle",
  "sku": "CANDLE-001",
  "stockLevel": 55,
  "unitType": "pieces",
  "unitPrice": 5.50,
  "minLevel": 10,
  "notes": "Vanilla scented",
  "retailPrice": 15.00,
  "wholesalePrice": 10.00,
  "ean": "1234567890123",
  "minimumQuantity": 1,
  "reorderURLs": ["https://example.com/reorder"],
  "locationStockLevels": {
    "Main Warehouse": 40,
    "Retail Store": 15
  },
  "productionType": "produced",
  "materials": [
    {
      "id": "mat_456",
      "name": "Soy Wax",
      "sku": "WAX-SOY",
      "unitType": "weight.lbs",
      "unitPrice": 3.25,
      "stockLevel": 100,
      "minLevel": 20,
      "quantityUsed": 0.5
    }
  ]
}
```

### Update Material Stock Level

```
POST /materials/:id/stock-level-updates
```

Updates the stock level of a material at a specific location.

**Request Body:**

```json
{
  "quantity": 50,
  "locationName": "Main Warehouse",
  "updateType": "restock",
  "notes": "Received shipment from supplier",
  "customStatusId": null
}
```

**Fields:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| quantity | number | Yes | The quantity to set or adjust |
| locationName | string | Yes | Name of the location |
| updateType | string | Yes | One of: `audit`, `restock`, `loss`, `custom` |
| notes | string | No | Optional notes about the update |
| customStatusId | number | Conditional | Required when updateType is `custom` |

**Update Types:**

- `audit`: Sets the stock level to the exact quantity specified (absolute value)
- `restock`: Adds the quantity to current stock level (adjustment)
- `loss`: Subtracts the quantity from current stock level (adjustment)
- `custom`: Uses a custom status (requires `customStatusId`)

**Response:**

```json
{
  "id": "mat_456",
  "name": "Soy Wax",
  "sku": "WAX-SOY",
  "stockLevel": 150,
  "unitType": "weight.lbs",
  "unitPrice": 3.25,
  "minLevel": 20,
  "notes": "Food grade soy wax",
  "retailPrice": null,
  "wholesalePrice": null,
  "ean": "9876543210987",
  "minimumQuantity": 10,
  "reorderURLs": ["https://supplier.com/wax"],
  "locationStockLevels": {
    "Main Warehouse": 130,
    "Production Floor": 20
  },
  "productionType": "supplied",
  "materials": []
}
```

### Get Locations

```
GET /locations
```

Returns a paginated list of locations.

**Query Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| limit | number | 100 | Maximum number of results (max: 1000) |
| offset | number | 0 | Number of results to skip |

**Response:**

```json
{
  "data": [
    {
      "id": "loc_789",
      "name": "Main Warehouse",
      "isDefault": true,
      "hideMaterials": false,
      "address1": "123 Storage St",
      "address2": "Unit 5",
      "city": "Portland",
      "state": "OR",
      "zip": "97201",
      "country": "US"
    },
    {
      "id": "loc_790",
      "name": "Retail Store",
      "isDefault": false,
      "hideMaterials": true,
      "address1": "456 Main St",
      "address2": null,
      "city": "Portland",
      "state": "OR",
      "zip": "97202",
      "country": "US"
    }
  ],
  "pagination": {
    "totalCount": 2,
    "offset": 0,
    "limit": 100
  }
}
```

## Type Definitions

### productionType

One of: `infinite`, `single`, `produced`, `supplied`, `bundle`

### unitType

One of:

- `pieces`
- `bundles` (read-only, cannot be set or changed)
- `cases`
- Weight: `weight.lbs`, `weight.oz`, `weight.kg`, `weight.grams`, `weight.mg`, `weight.ct`, `weight.gr`
- Length: `length.ft`, `length.yd`, `length.in`, `length.m`, `length.cm`, `length.mm`
- Area: `area.sqft`, `area.sqin`, `area.sqm`, `area.sqcm`
- Volume: `volume.oz`, `volume.pt`, `volume.qt`, `volume.ga`, `volume.ml`, `volume.liters`, `volume.bdft`, `volume.cuin`, `volume.cuft`, `volume.cuyd`, `volume.cm3`, `volume.m3`, `volume.tsp`, `volume.tbsp`, `volume.cup`
- Time: `time.seconds`, `time.minutes`, `time.hours`, `time.days`

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

**404 Not Found:**

```json
{
  "error": "Resource not found"
}
```

```json
{
  "error": "Location not found"
}
```

**400 Bad Request:**

```json
{
  "error": "quantity is required"
}
```

```json
{
  "error": "updateType must be one of: audit, restock, loss, custom"
}
```

```json
{
  "error": "Cannot update unitPrice when resource has stock blocks tied to non-initial logs"
}
```

