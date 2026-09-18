---
name: shopify-navigation-menus
description: Use when auditing, creating, updating, or validating Shopify native navigation menus for headers, mega menus, mobile navigation, footers, or legal links.
---

# Shopify Navigation Menus

Build navigation from Shopify's native Menu resources and real Collection/Page URLs. Maintain one navigation source of truth and verify both Admin data and theme rendering.

## Core Model

Treat navigation as two separate layers:

1. **Menu data:** Shopify `Menu` and nested menu items.
2. **Theme rendering:** Header, drawer, mega-menu, and Footer sections reading the selected menu settings.

A successful Menu API mutation proves only layer 1. Do not claim that Header or Footer is visible until layer 2 is inspected on the active theme and verified on the storefront.

## Workflow

1. Confirm the intended hierarchy, target menu handles, and whether theme changes are in scope.
2. Read all existing menus before writing. Record menu IDs, handles, titles, nested items, resource IDs, URLs, and current theme selections.
3. Resolve every internal destination to a real Shopify resource. Prefer Collection/Page resource links over hand-built relative URLs when the API supports them.
4. Build a canonical tree for each menu. Common responsibilities are:
   - `main-menu`: desktop Header, mobile drawer, and mega-menu taxonomy
   - `footer`: customer-service and discovery links
   - `legal`: policies and legal links
5. Validate tree depth, duplicate sibling titles, duplicate destinations, missing resources, stale handles, and unsupported menu item types.
6. Run payload tests locally before mutation.
7. Update existing menus by handle or ID. Create only handles that are absent. Never blindly create a second `main-menu` or `footer`.
8. Parse GraphQL errors and every `userErrors` entry.
9. Read all target menus back and compare the normalized tree exactly with the planned payload.
10. Inspect the active theme's Header and Footer section settings. Confirm the configured menu handles and recursive `link.links` rendering.
11. Verify desktop Header, mobile navigation, nested leaf links, Footer, and representative Collection pages on the live or preview storefront.

Read [references/menu-execution-workflow.md](references/menu-execution-workflow.md) before executing menu mutations.

## Required Invariants

- Shopify Menu data is the single navigation source; do not maintain a second hardcoded Liquid or JavaScript taxonomy.
- Leaf category items link directly to the corresponding Shopify Collection, not client-side filters.
- Collection references must resolve before the menu is written.
- Preserve stable menu handles and update by handle/ID.
- Use `section.settings.menu.links`, `link.links`, `link.object`, `link.url`, and `link.title` in theme implementations.
- Test desktop and mobile independently; they may use different markup while sharing the same menu.
- Verify Footer section presence separately from the existence of a `footer` menu.
- Do not invent promotional images, banners, descriptions, or Footer layout. Ask the user or reuse existing structured theme/metaobject content.
- Do not modify theme files when the request is limited to menu creation.

## User Handoffs

Ask the user to handle login, OAuth approval, 2FA, CAPTCHA, and interactive sales-channel controls. If Shopify Admin offers a quicker manual menu selector for choosing which menu a theme section uses, provide the exact section and handle to select. Once the user marks a handoff complete, do not repeat it unless explicitly asked to verify.

## Stop Conditions

Stop before writing when navigation scopes are missing, existing menus cannot be read, a target link has no valid resource, the requested hierarchy is ambiguous, or applying a payload would erase unknown existing links. Stop before theme edits when the desired Header/Footer layout has not been approved.

## Deliverables

Preserve:

- pre-write menu snapshot
- canonical menu specification
- validated mutation payloads
- raw mutation responses
- post-write menu snapshot
- exact tree comparison report
- theme binding audit
- storefront link-check report

Do not put store credentials, tokens, or hardcoded one-store GIDs into this reusable skill.
