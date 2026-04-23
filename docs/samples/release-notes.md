# PageTurner API Release Notes

## Version 1.2.0
**Release Date:** April 22, 2026

---

## Summary

This release introduces new filtering capabilities, performance improvements, and updates to request validation. It also includes breaking changes to parameter naming and pagination.

---

## New Capabilities

- Mood-based filtering via `mood` parameter  
- Author-based recommendations via `similar_to_author`  
- Expanded genre support:
  - Historical Fiction
  - Memoir
  - True Crime
  - Short Stories  

---

## Enhancements

- Improved response times (approximately 25% reduction in latency)  
- Updated ranking algorithm for improved recommendation accuracy  
- Standardized error response schema across endpoints  

---

## Breaking Changes

| Change | Description | Action Required |
|--------|------------|----------------|
| Parameter rename | `feeling` → `mood` | Update all requests to use `mood` |
| Pagination update | Page-based → Cursor-based | Update integration logic to support cursors |

---

## Bug Fixes

- Rating filters are now consistently applied  
- Duplicate recommendation issue resolved  
- External link validation corrected  

---

## Security Updates

- Strengthened request validation  
- Updated rate limiting thresholds  
- Improved handling of expired API keys  

---

## Deprecation Notice

The following features are deprecated and will be removed in a future release:

- `feeling` parameter (use `mood` instead)  
- Page-based pagination  

---

## Notes

This release focuses on improving recommendation quality and ensuring consistency across endpoints. Clients using deprecated fields should update their integrations ahead of future releases.

---