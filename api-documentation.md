---
title: ChannelRoute AI API Documentation
permalink: /api-documentation/
---

# ChannelRoute AI API Documentation

ChannelRoute AI provides API endpoints for creating and retrieving prospect records used in AI-assisted call-intelligence and outbound workflow applications.

This documentation covers the endpoints currently used by the ChannelRoute AI Zapier integration.

## Production Base URL

```text
https://channel-route-ai-call-intelligence-prototype.replit.app
```

## Authentication

The ChannelRoute AI Zapier integration uses API-key authentication.

Send the API key in the following HTTP request header:

```http
x-api-key: YOUR_API_KEY
```

Do not place API keys in URLs, query parameters, public repositories, or public documentation.

API credentials are issued directly by ChannelRoute AI.

## Authentication Test

Tests whether the supplied API key is valid.

### Endpoint

```http
GET /api/zapier/auth-test
```

### Example Request

```bash
curl \
  -H "x-api-key: YOUR_API_KEY" \
  "https://channel-route-ai-call-intelligence-prototype.replit.app/api/zapier/auth-test"
```

### Successful Response

Status:

```text
200 OK
```

Response:

```json
{
  "success": true,
  "app": "ChannelRoute AI"
}
```

### Invalid API Key Response

Status:

```text
401 Unauthorized
```

Response:

```json
{
  "success": false,
  "error": "Invalid API key"
}
```

### Server Configuration Error

Status:

```text
500 Internal Server Error
```

Response:

```json
{
  "success": false,
  "error": "Server API key is not configured"
}
```

## List Prospects

Returns the prospect records currently available in ChannelRoute AI.

This endpoint is used by the Zapier **New Prospect** polling trigger.

### Endpoint

```http
GET /api/prospects
```

### Example Request

```bash
curl \
  -H "x-api-key: YOUR_API_KEY" \
  "https://channel-route-ai-call-intelligence-prototype.replit.app/api/prospects"
```

### Successful Response

Status:

```text
200 OK
```

Example response:

```json
[
  {
    "id": 1,
    "contactName": "John Test",
    "company": "Test Roofing Company",
    "phone": "2145550123",
    "city": "Dallas",
    "state": "TX",
    "niche": "Roofing",
    "source": "zapier",
    "tags": [],
    "score": 75,
    "status": "ready",
    "notes": "",
    "businessSummary": ""
  }
]
```

Each prospect record includes a unique `id`. Zapier uses this identifier to distinguish new records from records it has already processed.

## Create Prospect

Creates a new prospect record in ChannelRoute AI.

This endpoint is used by the Zapier **Create Prospect** action.

### Endpoint

```http
POST /api/prospects
```

### Required Headers

```http
Content-Type: application/json
x-api-key: YOUR_API_KEY
```

### Request Fields

| Field | Type | Required | Description |
|---|---|---:|---|
| `company` | String | Yes | Company or organization name |
| `owner` | String | Yes | Prospect or contact name |
| `phone` | String | Yes | Prospect telephone number |
| `city` | String | Yes | Prospect city |
| `state` | String | No | State, province, or region |
| `niche` | String | No | Industry or business niche |
| `source` | String | No | Source that created the prospect |
| `tags` | Array | No | Tags associated with the prospect |
| `score` | Number | Yes | Prospect-priority score |

### Example Request

```bash
curl \
  -X POST \
  -H "Content-Type: application/json" \
  -H "x-api-key: YOUR_API_KEY" \
  -d '{
    "company": "Test Roofing Company",
    "owner": "John Test",
    "phone": "2145550123",
    "city": "Dallas",
    "state": "TX",
    "niche": "Roofing",
    "source": "zapier",
    "tags": [],
    "score": 75
  }' \
  "https://channel-route-ai-call-intelligence-prototype.replit.app/api/prospects"
```

### Successful Response

Status:

```text
201 Created
```

Example response:

```json
{
  "message": "Prospect created",
  "prospect": {
    "id": 4,
    "contactName": "John Test",
    "company": "Test Roofing Company",
    "phone": "2145550123",
    "city": "Dallas",
    "state": "TX",
    "niche": "Roofing",
    "source": "zapier",
    "tags": [],
    "score": 75,
    "status": "ready",
    "notes": "",
    "businessSummary": ""
  }
}
```

### Missing or Invalid Prospect Data

Status:

```text
400 Bad Request
```

Response:

```json
{
  "error": "Missing or invalid prospect data"
}
```

### Invalid JSON

Status:

```text
400 Bad Request
```

Response:

```json
{
  "error": "Invalid JSON"
}
```

## HTTP Status Codes

| Status | Meaning |
|---|---|
| `200` | Request completed successfully |
| `201` | Prospect created successfully |
| `400` | Missing, invalid, or malformed request data |
| `401` | Missing or invalid API key |
| `404` | Endpoint not found |
| `500` | Server configuration or internal server error |

## Zapier Integration

The ChannelRoute AI Zapier integration currently supports the following events.

### Trigger: New Prospect

Runs when a new prospect record becomes available in ChannelRoute AI.

```http
GET /api/prospects
```

### Action: Create Prospect

Creates a new ChannelRoute AI prospect from another application connected through Zapier.

```http
POST /api/prospects
```

## Security Requirements

- Use HTTPS for every production request.
- Send API keys only in the `x-api-key` request header.
- Do not place API keys in query strings.
- Do not publish API keys in repositories or documentation.
- Store API keys securely.
- Rotate exposed or compromised credentials immediately.

## Support

For API access, Zapier integration support, or documentation questions, contact:

```text
founder@channelroute.ai
```

## About ChannelRoute AI

ChannelRoute AI is an AI-assisted call-intelligence platform focused on live-human detection, identity-aware routing, prospect workflow management, and outbound communication optimization.

ChannelRoute AI was founded by Derek Allan Boman, an AI systems developer and inventor focused on communication systems, call intelligence, and workflow optimization.

Website:

```text
https://channelroute.ai
```
