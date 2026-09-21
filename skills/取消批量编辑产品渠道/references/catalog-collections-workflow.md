# Catalog And Collection Execution Reference

## 1. Preflight

Record the target `*.myshopify.com` domain and current authorization scopes. Use the authenticated Shopify CLI session or an approved Admin API token. Do not automate authentication forms, 2FA, or CAPTCHA.

Ask or infer from the active request which fields are writable. Default writable fields for classification work are tags, product type when clearly wrong or absent, taxonomy metafields explicitly selected by the user, and automated collections. Inventory and publication are separate workstreams.

## 2. Full Audit

Query products and collections with `pageInfo { hasNextPage endCursor }`. Continue until `hasNextPage` is false. A `first: 250` query is not proof that all records were read.

Capture for products:

- `id`, `handle`, `title`, `status`
- `vendor`, `productType`, `tags`
- description and relevant metafields
- variants only when SKU, option, or variant classification affects the rules

Capture for collections:

- `id`, `handle`, `title`
- collection type or rule sources
- product count and sort order

Save the unmodified API result with a timestamp before writing.

## 3. Classification Plan

Use hierarchical, namespaced tags. Example:

```text
department:cosmetics
category:lips
subcategory:lip-oils
merchandising:best-seller
```

Keep dimensions distinct. A product can belong to one primary department and multiple cross-cutting categories or merchandising groups. Document every rule as a predicate that can be reevaluated.

The plan should contain:

```json
{
  "productId": "gid://shopify/Product/...",
  "handle": "example",
  "preservedTags": ["existing-user-tag"],
  "managedTags": ["department:cosmetics", "category:lips"],
  "evidence": ["productType=Lip Oil", "title contains Lip Oil"],
  "reviewRequired": false
}
```

Ambiguous products use `reviewRequired: true`; do not silently force them into a category.

## 4. Collection Plan

For each proposed collection define:

- stable handle
- user-facing title
- all/any rule semantics
- exact conditions
- expected product IDs and expected count
- existing collection GID when updating

Match existing collections by handle first. Compare normalized rules before deciding whether an update is needed. Do not create duplicate handles or title-only duplicates.

## 5. Mutation Procedure

1. Generate payload files; do not hand-edit hundreds of API requests.
2. Validate uniqueness of product and collection GIDs.
3. Run a smoke mutation on one reversible, representative record.
4. Read that record back and independently verify it.
5. Execute the remaining rows through Shopify bulk operations when appropriate.
6. Save operation IDs and JSONL results.
7. Parse both top-level GraphQL errors and payload `userErrors`.

Bulk completion means the operation ran, not that every row succeeded. Count result rows and inspect each payload.

## 6. Independent Verification

After writes, query all products and collections again. Re-run the same deterministic predicates against the post-write snapshot. Report:

- product total and classified total
- collection planned/found/missing
- expected versus actual product count per collection
- rule mismatches
- empty collections
- duplicate handles
- failed mutation rows
- ambiguous products left for review

Use Admin API read-back as the source of truth for taxonomy and collection configuration.

## 7. Manual Handoffs

Prefer a user handoff when Shopify Admin provides a safer or much faster control:

- authenticate or approve scopes
- complete 2FA or CAPTCHA
- publish all selected products/collections to Online Store
- assign inventory quantities or inventory locations
- remove storefront password protection

Give the user a short checklist with exact object counts. When the user reports completion, mark the item `manual-complete` and continue. Do not repeat it through the API unless the user explicitly asks for API verification.

## 8. Storefront Interpretation

Keep these facts separate:

1. Collection exists.
2. Collection rules match products.
3. Collection is published to a sales channel.
4. Products are published to the same channel.
5. The storefront is publicly accessible and the theme renders the route.

A storefront redirect to `/password` is not a Collection 404. Inspect redirect history and Shopify headers before diagnosing publication failure.

## 9. Execution Report

Include dates, store, requested scope, counts, mutation operation IDs, failures, manual steps, and file locations. State what was not changed. Never include tokens, credentials, or passwords.
