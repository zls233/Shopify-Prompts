# Shopify Menu Execution Reference

## 1. Preflight And Scope

Typical Admin API scopes are `read_online_store_navigation` and `write_online_store_navigation`. Confirm access by reading menus before attempting writes. An authorization session existing does not prove it contains these scopes.

Determine separately whether the task includes:

- Menu data only
- Theme setting selection only
- Theme Liquid/JSON section changes
- Structured mega-menu content such as images or promo cards

Do not silently expand a menu-data task into theme code changes.

## 2. Audit Existing Menus

Read enough menu nodes to cover every menu and every nested item. Capture:

- menu `id`, `handle`, and `title`
- item `id`, `title`, `type`, `url`, and `resourceId`
- nested `items` recursively

Also inspect the active theme:

- `sections/header-group.json`
- Header section schema and selected menu setting
- `sections/footer-group.json`, if present
- Footer section schema and selected link-list settings
- mobile drawer implementation

Absence of `footer-group.json` or an equivalent Footer section can explain why a valid Footer menu does not render.

## 3. Canonical Menu Specification

Use a source-controlled JSON representation independent of Shopify response-only IDs. Example:

```json
{
  "handle": "main-menu",
  "title": "Main menu",
  "items": [
    {
      "title": "COSMETICS",
      "type": "COLLECTION",
      "resourceId": "gid://shopify/Collection/...",
      "items": [
        {
          "title": "Lips",
          "type": "COLLECTION",
          "resourceId": "gid://shopify/Collection/...",
          "items": []
        }
      ]
    }
  ]
}
```

Prefer resource-backed items for Shopify resources. Use direct URLs for external sites, search, account, or other destinations without a supported resource object.

## 4. Validate Before Write

Check:

- target handles are unique
- sibling titles are intentional and not duplicated
- all Collection/Page GIDs exist
- resource type matches the referenced GID
- hierarchy stays within Shopify/theme-supported depth
- no cycles exist in generated trees
- URLs are valid and internal URLs use consistent paths
- every intended leaf has a destination
- Header and mobile navigation can represent the same tree

Normalize planned and actual trees before comparison by ignoring response-only item IDs and normalizing empty child arrays.

## 5. Create Or Update Decision

Use this decision order:

1. Existing exact handle: update that menu.
2. No handle but an obvious uniquely matching legacy menu: report the proposed mapping before mutation.
3. Missing handle: create once.
4. Multiple possible matches: stop for user selection.

Never create a replacement before confirming the existing menu cannot be updated. Blind creation causes duplicates and leaves the theme bound to the old handle.

## 6. Mutation Procedure

1. Save the current menu snapshot.
2. Generate payload JSON from the canonical specification.
3. Validate the payload with a deterministic script or test.
4. Smoke-test one small missing menu or a reversible update when possible.
5. Execute updates and creates.
6. Save each response separately.
7. Require empty `userErrors` and no top-level GraphQL errors.

When using Shopify CLI:

```powershell
shopify store execute `
  --store example.myshopify.com `
  --query-file menu-update.graphql `
  --variable-file menu-update.variables.json `
  --allow-mutations `
  --json
```

Do not use a remembered input type without schema confirmation. Shopify API versions can distinguish types such as `MenuItemUpdateInput` and `MenuItemCreateInput`.

## 7. Read-Back Verification

Re-query by handle after every write. Compare:

- target menu count
- top-level item count
- total recursive node count
- titles and ordering
- item types
- resource IDs and URLs
- nested child structure

An exact normalized tree match is stronger evidence than a successful mutation response.

## 8. Theme Binding Verification

For Header, confirm the active section selects the expected handle and traverses nested links. A native pattern looks like:

```liquid
{% for link in section.settings.menu.links %}
  {{ link.title }}
  {% for childlink in link.links %}
    {{ childlink.title }}
  {% endfor %}
{% endfor %}
```

For Footer, confirm both:

- a Footer section or section group exists in the active theme
- its link-list settings point to `footer`, `legal`, or the intended handles

If menu data exists but the section is absent, report `menu-written/theme-not-rendering`. Do not invent a Footer layout unless theme changes are approved.

## 9. Storefront Verification

Check at least:

- each top-level Header link
- one nested branch at every supported depth
- all Collection leaf URLs
- desktop Header expansion
- mobile drawer expansion and close behavior
- Footer and legal links
- Collection page product grid and product navigation

Distinguish redirects to `/password` from 404 responses. Record final URLs, response codes, and visible page identity.

## 10. Final Report

Report per menu:

- created or updated
- top-level and total recursive nodes
- exact read-back match
- unresolved links
- selected theme section setting
- desktop/mobile/Footer rendering result
- manual actions remaining

State clearly whether success applies to Menu data, theme binding, storefront rendering, or all three.
