Testing this page

# Payments API Reference

!!! note "Context"
    **Audience:** Backend developers integrating a payments API for the first time.  
    **Goal:** Give developers everything they need to make a successful API call without leaving the docs.  
    **Constraints:** Needed to cover authentication, core endpoints, and error handling in a single reference page without overwhelming.

---

## Authentication

All requests must include your API key in the `Authorization` header using Bearer token format. Keys are environment-scoped — use test keys (`sk_test_`) during development and live keys (`sk_live_`) in production only.

```
Authorization: Bearer sk_live_••••••••••••••••
```

!!! warning
    Never expose API keys in client-side code or version control. Store them in environment variables.

---

## Base URL

```
https://api.paymentsco.example/v2
```

---

## POST /charges

Creates a new payment charge against a payment method token. Returns a `Charge` object on success.

### Request body

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `amount` | integer | Yes | Amount in the smallest currency unit (e.g. cents). Minimum: 50. |
| `currency` | string | Yes | Three-letter ISO 4217 currency code, e.g. `usd`. |
| `source` | string | Yes | Payment method token from the client SDK. |
| `description` | string | No | Freeform string for your records. Max 500 characters. |

### Example request

```bash
curl https://api.paymentsco.example/v2/charges \
  -H "Authorization: Bearer sk_live_..." \
  -H "Content-Type: application/json" \
  -d '{
    "amount": 2000,
    "currency": "usd",
    "source": "tok_visa",
    "description": "Order #1234"
  }'
```

### Example response

```json
{
  "id": "ch_3NxKaB2eZvKYlo2C",
  "object": "charge",
  "amount": 2000,
  "currency": "usd",
  "status": "succeeded",
  "created": 1711234567,
  "description": "Order #1234"
}
```

---

## GET /charges/:id

Retrieves a previously created charge by its ID.

### Path parameters

| Parameter | Description |
|-----------|-------------|
| `id` | The charge ID, e.g. `ch_3NxKaB2eZvKYlo2C`. |

### Example request

```bash
curl https://api.paymentsco.example/v2/charges/ch_3NxKaB2eZvKYlo2C \
  -H "Authorization: Bearer sk_live_..."
```

---

## Error codes

| Code | HTTP status | Description |
|------|-------------|-------------|
| `invalid_request` | 400 | Missing or malformed request parameter. |
| `authentication_error` | 401 | Invalid or missing API key. |
| `card_declined` | 402 | The card was declined by the issuer. |
| `rate_limit_error` | 429 | Too many requests — back off and retry. |
| `server_error` | 500 | Unexpected server error. Contact support. |
