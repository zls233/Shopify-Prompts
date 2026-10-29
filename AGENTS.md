# Shopify Prompt Routing

## FAST 核心交付、商品 ACTIVE/9999、Footer 八页（新默认规则）

先读取 `STAGE_COMPLETION_CONTRACT.md` v2。FAST 先完成核心 P0/P1，非阻断 P1/P2 可 deferred；仅当有对应需要时读取子 Prompt 的适用章节，不在进入阶段时把全部长文件及所有专项一次性读入上下文。用批量与断点处理长任务；关键外部阻断时列失败清单，不无限重试。默认所有目标店铺商品 `ACTIVE`、可跟踪库存 9999；Footer 自动复用 Shopify-Template 八份页面内容模板。


## 内容缺失自动补齐（默认行为）

**不要因为原站没有相应页面、店铺没有现成文案或用户未事先提供稿件而停止。** 在本次指定的建站范围内，优先复用已存在的素材与资源；缺少 About Us、Contact Us、Footer 八个信息页或必要的非敏感 UI 文案时，Codex 应自主设计、撰写、创建、绑定并验证，不反复询问用户。适用详规见 `05-customization/5-2-功能削减.md` 和 `03-theme/pages/3-pages-6-页面补齐.md`。

原创描述和文案可以合理补全缺失的展示内容；价格、商品属性、评论、商家法定身份、实际配送/退货条件、个人信息处理承诺等必须以真实数据为准。缺少具体业务事实时采用通用不作额外承诺的表达，把后续业务核定记录为备注，而非把页面技术实现整体标记 blocked。仍遵守既有 Draft/Live、写入权限、支付与凭据边界。


## FAST 完成状态（高于旧严格验收语句）

统一遵守 `STAGE_COMPLETION_CONTRACT.md` v2。默认 FAST 以核心交付完成为标准，可标 `completed_core`，将 P2 / 不影响核心交付的 P1 记 `deferred`；仅 P0/关键数据和当前用户明确要求尚未满足时为 `partial/blocked_core`。禁止为了表面 100% 把 `not_run` 写作 `verified`。小清单、一次验证、只处理差集；不再将所有子 Prompt 的增强性验收叠加为阶段硬门槛。

## 商品分类前台核验（强制闭环）

商品分类以完整导航映射和成员数据批量对账为主，浏览器只对核心分类样本、不同层级和风险项实测桌面与手机；已变化或有异常的分类针对性复验。用户明确要求全分类逐项 UI 测试或 FULL 时才逐分类打开浏览器。错误分类、主要链接 404、核心商品不可见仍需修复；不得声称没有实际检查的分类已完成 UI 验收。

**Default storefront scope:** Before reference capture, theme, content pages, footer or QA, read `05-customization/5-2-功能削减.md` as the only scope authority. Announcement Bar is always exactly one `Free Shipping on All Orders` message. This is a theme-display-only requirement: never change or audit actual shipping profiles, rates, zones, Markets, or custom-checkout shipping based on this banner request. Do not claim actual shipping fees are zero. Footer has precisely CUSTOMER CARE (Privacy Policy, Return Policy, Shipping Policy, Service Policy) and ORDER INFO (About Us, Contact Us, Terms Of Purchase, Terms Of Use), with no social entries, Newsletter or extra columns. All eight labels must target real, accessible Policy/Page resources. Reuse verified store content first; when missing, autonomously draft original, neutral, store-appropriate content and create/bind the missing pages without asking the user for copy. Keep unverified factual claims out of the copy; note any later merchant/legal review separately instead of blocking technical completion. Other independent FAQ/Stores/Blogs/Articles/Editorial pages are excluded unless the current project specifically requires them. Keep Home/PLP/PDP/Cart and real checkout-entry QA mandatory. Stage prompts must not re-introduce broader legacy source-site content. Update the project memory with any explicit exception.



During FAST 1-1 initialization, automatically acquire an enabled store's current Storefront Password: try supported CLI/API read capabilities, then use the authenticated Shopify Admin browser UI when necessary. Save only to ignored project-root `.env` keys `SHOPIFY_STORE_DOMAIN` and `SHOPIFY_STOREFRONT_PASSWORD`, preserve existing entries, and verify access without logging the password. Do not change protection settings. An inaccessible UI is `blocked`, not permission to ask for a manual password. See `01-project/1-1-项目初始化与授权验证-阶段入口.md` and `EXECUTION_RULES.md`.

For end-to-end unattended stage orchestration, `运行0-1` resolves to `00-autopilot/0-1-全流程建站与断点续跑-总入口.md`. It runs the six existing FAST phase entry points sequentially with verification, checkpoint/resume and explicit authorization boundaries; it never grants Live publication or store opening authority.

Default `EXECUTION_MODE=FAST`: `1-1`, `2-1`, `4-1`, `3-1`, `5-1`, and `6-1` are complete phase entry points. Read only the relevant subordinate Prompt sections when the step is needed; perform and verify core scope in the same run; do not ask the user to issue each following substep. Run subordinate prompts separately only for actual failures, special precision, or explicit `FULL` mode. `4-1` includes CSV cleanup, full import and collections; `3-1` includes necessary Theme implementation and Draft browser QA; `5-1` includes removals and Cookie; `6-1` includes risk-based `6-6` programmatic catalog comparison plus sampled UI, and `6-5` core-link delta scan/fix/retest on the authorized Draft and checkout-entry test after separately authorized release/public opening. Prompt execution cannot select the model or reasoning effort. For routine FAST tasks use an adequate efficient effort; reserve XHigh for complex bugs or deep architecture when needed, and respect the operator's chosen setting. FAST does not authorize publishing Live, disabling password protection, changing merchant address, or modifying payments.


When the user says `运行2-1`, `运行3-pages-2`, or another numbered command,
resolve and execute the corresponding Markdown file in this repository. Read
`PROMPT_ROUTING.md` and `EXECUTION_RULES.md` first. The number is a file
selector, not a task summary.

Match the complete identifier, including `pages`, before using the phase
number. Require exactly one file match; if none or several match, report that
ambiguity instead of guessing. Read the selected Prompt, then execute it in
the current Shopify project using that project's `PROJECT_MEMORY.md` and the
Prompt's store, authorization, Draft/Live, and verification requirements.

For example, `4-1` is `04-catalog/4-1-商品采集清洗导入与分类-阶段入口.md`, while `3-pages-2` is
`03-theme/pages/3-pages-2-顶栏与分类展开.md`. Do not substitute a previous audit or an
inferred workflow for the selected file.

For continuations, recover the selected Prompt and verified progress from the
current project. Do not rerun completed imports or treat old task numbers,
sample data, successful uploads, or empty-catalog checks as full completion.
Apply the execution rules to bundled skills as well as numbered Prompts.

Do not replicate reference-site checkout/payment/order-confirmation pages. Preserve the actual Cart → Checkout action for the project's approved checkout destination. After publishing and public storefront verification, run `06-qa/6-4-购物车与结账入口测试.md`; stop upon reaching the checkout page without entering user/payment information or submitting an order.


Numbered Prompt filenames start with the complete command identifier (`1-1-`, `4-3-`, `3-pages-2-`). Match that exact prefix, not a local numeric basename.
