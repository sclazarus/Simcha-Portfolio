# Release Notes — v3.4.0

*Released 14 March 2025*

!!! note "Context"
    **Audience:** Developers and platform operators who maintain integrations and need to know what changed and whether action is required.  
    **Goal:** Communicate changes clearly, flag breaking changes prominently, and give readers a direct path to act.  
    **Constraints:** This release included a breaking change affecting ~30% of API users — needed careful framing that didn't bury the lead.

---

!!! danger "Breaking change"
    The `charges.create` endpoint no longer accepts raw card data. Migrate to tokenised payment methods before upgrading to v3.4.0. See the [migration guide](#).

---

## What's new

### Instant payouts

Funds can now be paid out to supported debit cards within 30 minutes of a successful charge. Enable instant payouts in **Settings → Payouts**. An additional fee applies per transaction — see [Payout pricing](#) for details.

### Webhook retry dashboard

Failed webhook deliveries are now visible in the dashboard under **Settings → Webhooks → Failed deliveries**. You can inspect the full request and response payload and trigger a manual retry.

### Idempotency key support

All `POST` endpoints now accept an `Idempotency-Key` header. Include a unique key to safely retry requests without creating duplicate charges. Keys expire after 24 hours.

```http
POST /v2/charges
Idempotency-Key: a8098c1a-f86e-11da-bd1a-00112444be1e
```

---

## Breaking changes

### `charges.create`: raw card data removed

The following parameters have been removed from `POST /charges`:

- `card_number`
- `exp_month`
- `exp_year`
- `cvc`

**Action required:** Use a tokenised `source` obtained from the client-side SDK instead. See the [migration guide](#) for step-by-step instructions and a comparison of before/after request shapes.

---

## Deprecations

### `/v1/` base URL

The v1 API base URL is deprecated as of this release and will be removed in **v4.0.0 (Q4 2025)**. Migrate all requests to the `/v2/` base URL.

---

## Bug fixes

- Fixed: Webhook signatures occasionally failed validation when the payload contained multi-byte UTF-8 characters.
- Fixed: Pagination cursor returned incorrect results when filtering charges by `status=failed`.
- Fixed: Dashboard session timeout did not refresh correctly when using SSO.
