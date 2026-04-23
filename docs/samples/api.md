# PageTurner API

Book Recommendation API — Developer Documentation

**Version:** 1.1.0 · **Base URL:** `https://api.pageturner.io/v1` · **Format:** JSON

---

## Overview

The PageTurner API lets you build book recommendation features into any application. Clients submit structured reader preferences such as genre, mood, page count, minimum rating, and optional free-text context, and the API returns ranked book recommendations with metadata and match explanations.

Use cases include:

- Reading apps with personalized recommendations
- Library discovery tools
- E-commerce product discovery
- Chatbot and voice-assistant reading features

---

## Authentication

All requests must include a valid API key as a Bearer token in the `Authorization` header.

```http
Authorization: Bearer YOUR_API_KEY
```

API keys are available from the PageTurner Developer Portal.

### Access tiers

| Tier | Rate limit | Intended use |
|------|------------|--------------|
| Free | 100 requests / day | Personal projects and prototyping |
| Reader | 5,000 requests / day | Small production applications |
| Librarian | Unlimited | Enterprise and high-volume integrations |

### Authentication errors

If the API key is missing, invalid, or expired, the API returns:

```json
{
  "error": {
    "code": "UNAUTHORIZED",
    "message": "Missing or invalid API key.",
    "status": 401
  }
}
```

---

## Request and Response Conventions

### Content type

All request bodies must be sent as `application/json`.

### Success envelope

Successful responses use the following structure:

```json
{
  "data": [],
  "meta": {
    "total_results": 4,
    "limit": 4,
    "offset": 0,
    "has_more": false,
    "query_time_ms": 287,
    "request_id": "req_01JQ8J8Y7J6R7YFQZX4M3M9H2A"
  }
}
```

### Error envelope

Error responses use the following structure:

```json
{
  "error": {
    "code": "INVALID_PARAMS",
    "message": "Validation failed.",
    "status": 422,
    "details": [
      {
        "field": "min_rating",
        "issue": "Must be a number between 0 and 5."
      }
    ],
    "request_id": "req_01JQ8J8Y7J6R7YFQZX4M3M9H2A"
  }
}
```

### Request IDs

Every response includes a `request_id` value. Log this value when contacting support.

---

## Endpoints

### Get recommendations

```http
POST /recommendations
```

Returns ranked book recommendations based on one or more filter fields.

**Full URL:** `https://api.pageturner.io/v1/recommendations`

At least one filter field is required.

#### Request body

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `genres` | array of strings | No | Genre filters. Accepted values are listed in [`GET /genres`](#list-supported-genres). |
| `moods` | array of strings | No | Mood filters. Accepted values are listed in [`GET /moods`](#list-supported-moods). |
| `min_pages` | integer | No | Minimum page count. Must be `>= 1`. |
| `max_pages` | integer | No | Maximum page count. Must be `>= min_pages`. |
| `min_rating` | number | No | Minimum Goodreads rating. Must be between `0` and `5`. Example: `4.0`. |
| `query` | string | No | Free-text reading intent or comparison prompt. Maximum 500 characters. |
| `limit` | integer | No | Number of recommendations to return. Range: `1-10`. Default: `4`. |
| `offset` | integer | No | Number of results to skip for pagination. Minimum: `0`. Default: `0`. |
| `sort` | string | No | Result ordering. Accepted values: `relevance`, `rating`, `publication_year`. Default: `relevance`. |

#### Validation rules

- At least one of `genres`, `moods`, `min_pages`, `max_pages`, `min_rating`, or `query` must be present.
- Empty arrays are not allowed.
- `max_pages` must be greater than or equal to `min_pages`.
- `query` must not exceed 500 characters.
- Unsupported enum values return `422 INVALID_PARAMS`.

#### Example request

```http
POST /v1/recommendations
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json

{
  "genres": ["historical-fiction", "literary-fiction"],
  "moods": ["slow-burn", "emotional"],
  "min_pages": 400,
  "max_pages": 600,
  "min_rating": 4.0,
  "query": "Something like Donna Tartt but set outside the US, preferably Japan or Europe.",
  "limit": 4,
  "offset": 0,
  "sort": "relevance"
}
```

#### Response fields

| Field | Type | Description |
|-------|------|-------------|
| `id` | string | Stable PageTurner book identifier |
| `title` | string | Book title |
| `author` | string | Author full name |
| `publication_year` | integer | Year of first publication |
| `page_count` | integer | Estimated page count |
| `goodreads_rating` | number | Average Goodreads rating |
| `goodreads_url` | string | Goodreads URL |
| `genres` | array of strings | Canonical genre values |
| `moods` | array of strings | Canonical mood values |
| `summary` | string | Short editorial summary |
| `why_matched` | string | Explanation of why the title matched the request |

#### Example response

```json
{
  "data": [
    {
      "id": "book_7wYk29fM",
      "title": "The Master and Margarita",
      "author": "Mikhail Bulgakov",
      "publication_year": 1967,
      "page_count": 384,
      "goodreads_rating": 4.32,
      "goodreads_url": "https://www.goodreads.com/book/show/117833",
      "genres": ["literary-fiction", "historical-fiction", "classic"],
      "moods": ["dark-atmospheric", "thought-provoking"],
      "summary": "A satirical novel set in Soviet Moscow, where the Devil arrives and disrupts literary, political, and spiritual life.",
      "why_matched": "Matched for literary tone, European setting, strong critical reputation, and a rating above 4.0."
    }
  ],
  "meta": {
    "total_results": 14,
    "limit": 4,
    "offset": 0,
    "has_more": true,
    "query_time_ms": 287,
    "request_id": "req_01JQ8J8Y7J6R7YFQZX4M3M9H2A"
  }
}
```

#### Empty result behavior

If the request is valid but no books match the filters, the API returns `200 OK` with an empty `data` array.

```json
{
  "data": [],
  "meta": {
    "total_results": 0,
    "limit": 4,
    "offset": 0,
    "has_more": false,
    "query_time_ms": 109,
    "request_id": "req_01JQ8JFBJZXQ3WQFGH4W2B7Y2F"
  }
}
```

---

### List supported genres

```http
GET /genres
```

Returns the full list of supported genre values for request validation and UI dropdowns.

#### Example response

```json
{
  "data": {
    "genres": [
      "literary-fiction",
      "science-fiction",
      "fantasy",
      "mystery-thriller",
      "historical-fiction",
      "romance",
      "non-fiction",
      "horror",
      "biography",
      "classic"
    ]
  },
  "meta": {
    "request_id": "req_01JQ8JH4D3VGX2P6M1W0Z5K4Q1"
  }
}
```

---

### List supported moods

```http
GET /moods
```

Returns the full list of supported mood values for request validation and UI dropdowns.

#### Example response

```json
{
  "data": {
    "moods": [
      "feel-good",
      "dark-atmospheric",
      "fast-paced",
      "slow-burn",
      "thought-provoking",
      "witty",
      "emotional"
    ]
  },
  "meta": {
    "request_id": "req_01JQ8JHXC7F6H1D1T9XQ3R0Y5V"
  }
}
```

---

## Error Codes

| Code | Status | Description |
|------|--------|-------------|
| `UNAUTHORIZED` | 401 | Missing, invalid, or expired API key |
| `FORBIDDEN` | 403 | Your plan does not permit the request |
| `INVALID_PARAMS` | 422 | Request body failed validation |
| `RATE_LIMITED` | 429 | You exceeded your request quota |
| `SERVER_ERROR` | 500 | Internal error |

### Validation error example

```json
{
  "error": {
    "code": "INVALID_PARAMS",
    "message": "Validation failed.",
    "status": 422,
    "details": [
      {
        "field": "max_pages",
        "issue": "Must be greater than or equal to min_pages."
      },
      {
        "field": "genres[0]",
        "issue": "Unsupported genre value 'Historical Fiction'. Use canonical slug values from GET /genres."
      }
    ],
    "request_id": "req_01JQ8JKERQ5YV9DRQJ28KJ3B8N"
  }
}
```

---

## Rate Limits

Rate limits are enforced per API key and plan.

When a rate limit is exceeded, the API returns `429 Too Many Requests`.

```json
{
  "error": {
    "code": "RATE_LIMITED",
    "message": "You have exceeded your plan's request quota.",
    "status": 429,
    "request_id": "req_01JQ8JM2FQ1N1K7S5A2A6R1D4M"
  }
}
```

---

## Sorting and Ranking

The `sort` field controls result ordering:

| Value | Description |
|-------|-------------|
| `relevance` | Best semantic and filter match to the request |
| `rating` | Highest Goodreads rating first |
| `publication_year` | Most recently published titles first |

If `sort` is omitted, results are returned in `relevance` order.

---

## Data Source Notes

Recommendation metadata is sourced from Goodreads and PageTurner editorial processing.

Clients should not assume Goodreads values are real-time. Ratings, URLs, and metadata may be cached and refreshed periodically.

---

## Quick Start

The example below fetches four fast-paced fantasy recommendations with a minimum rating of 4.0.

```bash
curl -X POST https://api.pageturner.io/v1/recommendations \
  -H 'Authorization: Bearer YOUR_API_KEY' \
  -H 'Content-Type: application/json' \
  -d '{
    "genres": ["fantasy"],
    "moods": ["fast-paced"],
    "min_rating": 4.0,
    "limit": 4,
    "sort": "relevance"
  }'
```

---

## Integration Notes

For production clients:

- cache `/genres` and `/moods`
- log `request_id` values for debugging
- handle `429` and `5xx` responses with retry logic
- treat empty recommendation lists as valid successful responses
- use canonical slug values from lookup endpoints rather than hard-coding display labels



