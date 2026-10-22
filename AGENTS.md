# Shopify Prompt Routing

**Default storefront scope:** Before reference capture, theme, content pages, footer or QA, read `05-customization/5-2-功能削减.md` as the only scope authority. Announcement Bar is always exactly one `Free Shipping on All Orders` message. This is a theme-display-only requirement: never change or audit actual shipping profiles, rates, zones, Markets, or custom-checkout shipping based on this banner request. Do not claim actual shipping fees are zero. Footer has precisely CUSTOMER CARE (Privacy Policy, Return Policy, Shipping Policy, Service Policy) and ORDER INFO (About Us, Contact Us, Terms Of Purchase, Terms Of Use), with no social entries, Newsletter or extra columns. All eight labels must target real approved Policy/Page content; never fabricate legal copy, contact details or shipping promises. Other independent FAQ/Stores/Blogs/Articles/Editorial pages are excluded unless the current project specifically requires them. Keep Home/PLP/PDP/Cart and real checkout-entry QA mandatory. Stage prompts must not re-introduce broader legacy source-site content. Update the project memory with any explicit exception.



During FAST 1-1 initialization, automatically acquire an enabled store's current Storefront Password: try supported CLI/API read capabilities, then use the authenticated Shopify Admin browser UI when necessary. Save only to ignored project-root `.env` keys `SHOPIFY_STORE_DOMAIN` and `SHOPIFY_STOREFRONT_PASSWORD`, preserve existing entries, and verify access without logging the password. Do not change protection settings. An inaccessible UI is `blocked`, not permission to ask for a manual password. See `01-project/1-1-创建项目.md` and `EXECUTION_RULES.md`.

For end-to-end unattended stage orchestration, `运行0-1` resolves to `00-autopilot/0-1-全流程自动执行.md`. It runs the six existing FAST phase entry points sequentially with verification, checkpoint/resume and explicit authorization boundaries; it never grants Live publication or store opening authority.

Default `EXECUTION_MODE=FAST`: `1-1`, `2-1`, `4-1`, `3-1`, `5-1`, and `6-1` are complete phase entry points. Read their subordinate Prompt instructions and execute, validate, and record all necessary work in the same run; do not ask the user to issue each following substep. Run subordinate prompts separately only for actual failures, special precision, or explicit `FULL` mode. `4-1` includes CSV cleanup, full import and collections; `3-1` includes necessary Theme implementation and Draft browser QA; `5-1` includes removals and Cookie; `6-1` includes mandatory `06-qa/6-5-全站入口NotFound扫描与修复.md` link audit/fix/retest on the authorized Draft and checkout-entry test after separately authorized release/public opening. Prompt execution cannot select the model: choose GPT-6.1-Sol XHigh in Codex beforehand. FAST does not authorize publishing Live, disabling password protection, changing merchant address, or modifying payments.


When the user says `运行2-1`, `运行3-pages-2`, or another numbered command,
resolve and execute the corresponding Markdown file in this repository. Read
`PROMPT_ROUTING.md` and `EXECUTION_RULES.md` first. The number is a file
selector, not a task summary.

Match the complete identifier, including `pages`, before using the phase
number. Require exactly one file match; if none or several match, report that
ambiguity instead of guessing. Read the selected Prompt, then execute it in
the current Shopify project using that project's `PROJECT_MEMORY.md` and the
Prompt's store, authorization, Draft/Live, and verification requirements.

For example, `4-1` is `04-catalog/4-1-爬取商品SKU.md`, while `3-pages-2` is
`03-theme/pages/3-pages-2-顶栏与分类展开.md`. Do not substitute a previous audit or an
inferred workflow for the selected file.

For continuations, recover the selected Prompt and verified progress from the
current project. Do not rerun completed imports or treat old task numbers,
sample data, successful uploads, or empty-catalog checks as full completion.
Apply the execution rules to bundled skills as well as numbered Prompts.

Do not replicate reference-site checkout/payment/order-confirmation pages. Preserve the actual Cart → Checkout action for the project's approved checkout destination. After publishing and public storefront verification, run `06-qa/6-4-购物车与结账入口测试.md`; stop upon reaching the checkout page without entering user/payment information or submitting an order.


Numbered Prompt filenames start with the complete command identifier (`1-1-`, `4-3-`, `3-pages-2-`). Match that exact prefix, not a local numeric basename.
