---
name: shopify-catalog-collections
description: Use when auditing a Shopify catalog, defining product taxonomy, applying classification tags, or creating and validating automated collections from product data.
---

# Shopify Catalog And Collections

Build a maintainable chain from product classification to Shopify automated collections. Prefer native Shopify data and auditable, rerunnable artifacts over theme code or manually curated product IDs.

## Operating Boundary

Split the work by who can perform it most reliably:

| Agent executes | User executes in Shopify Admin |
|---|---|
| Read-only catalog audit and pagination | Login, OAuth approval, 2FA, CAPTCHA |
| Taxonomy proposal and classification rules | Assigning Online Store sales channels when bulk Admin UI is faster |
| Deterministic product tag/product type/metafield updates | Inventory quantity changes unless explicitly requested |
| Automated Collection create/update by stable handle | Storefront password removal or channel availability settings |
| API read-back, count checks, duplicates, failed-row report | Any step the user marks as manually completed or abandoned |

Never redo a user-completed manual step. Record it as an external prerequisite and continue with the remaining validation.

## Workflow

1. Confirm store, scope, desired taxonomy source, and fields that must not change.
2. Audit all products and existing collections with pagination. Save raw responses before mutation.
3. Normalize products by stable Shopify GID and handle. Report duplicate handles, duplicate SKUs, missing values, statuses, and existing taxonomy fields.
4. Draft taxonomy rules from title, product type, tags, vendor, description, and existing metafields. Rules must be explainable and deterministic.
5. Produce a plan before writing: each product's preserved tags, managed tags, classification evidence, and target collections.
6. Validate a small sample across ambiguous and representative categories.
7. Update products without removing unrelated existing tags. Prefer namespaced tags such as `department:cosmetics`, `category:lips`, and `subcategory:lip-oils`.
8. Create or update automated collections by handle. Never create a duplicate merely because the title differs in capitalization.
9. Read back all products and collections. Recompute expected membership independently and compare expected versus actual counts.
10. Hand off sales-channel publication to the user when it is faster or requires interactive Admin work. Do not block the taxonomy audit on this step.
11. Save an execution audit with successful rows, failed rows, mismatches, empty collections, skipped manual steps, and rerun instructions.

Read [references/catalog-collections-workflow.md](references/catalog-collections-workflow.md) before executing mutations.

## Required Invariants

- Fetch every page; never infer the catalog from the first page.
- Preserve user-owned tags and metafields unless removal is explicitly requested.
- Do not invent classification when evidence is insufficient. Put ambiguous products in a review queue.
- Automated Collection membership must use tags, product type, category, or metafields, not hardcoded product IDs in theme code.
- Use stable, readable handles and update existing matching handles.
- Run one mutation smoke test before bulk writes.
- Parse every mutation response and every `userErrors` entry.
- Verify with a fresh Admin API read, not only mutation success output.
- Treat storefront publication separately from Collection existence and rule correctness.
- Never write inventory, price, barcode, SKU, or sales-channel state unless the user explicitly keeps that item in scope.

## Stop Conditions

Stop and report without guessing when authentication, 2FA, CAPTCHA, missing scopes, ambiguous taxonomy policy, destructive tag replacement, or an unexpected live-store mismatch requires user judgment. If the user says a step is manual, complete, abandoned, or out of scope, do not retry it.

## Deliverables

At minimum preserve:

- raw catalog snapshot
- normalized catalog or taxonomy plan
- mutation payloads
- per-row mutation results
- post-write catalog snapshot
- collection membership audit
- execution report including manual handoffs

Do not place credentials, access tokens, passwords, or one-store-only IDs in reusable skills or reports.
