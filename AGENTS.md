# Shopify Prompt Routing

## 内容缺失自动补齐（默认行为）

**不要因为原站没有相应页面、店铺没有现成文案或用户未事先提供稿件而停止。** 在本次指定的建站范围内，优先复用已存在的素材与资源；缺少 About Us、Contact Us、Footer 八个信息页或必要的非敏感 UI 文案时，Codex 应自主设计、撰写、创建、绑定并验证，不反复询问用户。适用详规见 `05-customization/5-2-功能削减.md` 和 `03-theme/pages/3-pages-6-页面补齐.md`。

原创描述和文案可以合理补全缺失的展示内容；价格、商品属性、评论、商家法定身份、实际配送/退货条件、个人信息处理承诺等必须以真实数据为准。缺少具体业务事实时采用通用不作额外承诺的表达，把后续业务核定记录为备注，而非把页面技术实现整体标记 blocked。仍遵守既有 Draft/Live、写入权限、支付与凭据边界。


## 任务完成闭环（入口强制规则）

本仓库已有详细执行规范，**以 `EXECUTION_RULES.md` 的“任务完成闭环”章节为唯一详细规则来源**；本节只负责在 Codex 进入仓库时强制加载与执行，不另建相互矛盾的验收标准。

- 接到任何编号命令（含 FAST / FULL / AUTOPILOT）时，先读取 `PROMPT_ROUTING.md`、`EXECUTION_RULES.md`、目标编号 Prompt、适用的子 Prompt，以及当前实际项目的 `AGENTS.md` 和 `PROJECT_MEMORY.md`。不得只凭编号或历史摘要推断执行范围。
- 在动手前将全部适用要求和验收条件展开为逐项任务清单，并持久化到**当前 Shopify 项目**的 `TASK_CHECKLIST.md`（如已有等价文件则沿用）。逐项保存状态、真实证据、缺口和下一步；不要将项目状态写进通用 Prompts 仓库。
- 每个任务必须“执行 → 验证 → 定位差异 → 修复 → 复验”，直到适用项全部验证通过。创建脚本、下载部分素材、产生截图、检查 JSON、工具返回成功、上传主题或仅完成样本，都不能单独作为整阶段完成证明。
- 遇到可修复失败，在现有授权范围内继续完成未解决项；对临时错误有限重试，对确定性错误调整处理方式，对批量任务只处理差集。不要一次失败就提前总结，也不要无变化地无限重试。
- 如果某项因外部权限、登录、数据不可访问或其他真实依赖无法继续，先处理所有独立可执行项，再将其记为 `blocked` 并保留可复现证据和精确恢复入口。不可擅自发布 Live、修改支付、关闭密码保护或扩大测试权限以达到“完成”。
- 最终核对“原始 Prompt 全部适用要求 ↔ 清单 ↔ 实际验证证据”。只有全部适用项为 `verified`，且其余项有明确依据为 `excluded`，才报告 `completed`。任何 `failed / blocked / not_run` 均不得冒充通过；真实报告 `partial` 或 `blocked`，并同步 `PROJECT_MEMORY.md`。
- 对于“继续”“重试”或新会话，优先核验已保存证据和当前远端状态，接着处理剩余项；不得重复已完成的导入、授权操作或批量写入。FAST 只减少手动触发次数，不降低验收标准。


**Default storefront scope:** Before reference capture, theme, content pages, footer or QA, read `05-customization/5-2-功能削减.md` as the only scope authority. Announcement Bar is always exactly one `Free Shipping on All Orders` message. This is a theme-display-only requirement: never change or audit actual shipping profiles, rates, zones, Markets, or custom-checkout shipping based on this banner request. Do not claim actual shipping fees are zero. Footer has precisely CUSTOMER CARE (Privacy Policy, Return Policy, Shipping Policy, Service Policy) and ORDER INFO (About Us, Contact Us, Terms Of Purchase, Terms Of Use), with no social entries, Newsletter or extra columns. All eight labels must target real, accessible Policy/Page resources. Reuse verified store content first; when missing, autonomously draft original, neutral, store-appropriate content and create/bind the missing pages without asking the user for copy. Keep unverified factual claims out of the copy; note any later merchant/legal review separately instead of blocking technical completion. Other independent FAQ/Stores/Blogs/Articles/Editorial pages are excluded unless the current project specifically requires them. Keep Home/PLP/PDP/Cart and real checkout-entry QA mandatory. Stage prompts must not re-introduce broader legacy source-site content. Update the project memory with any explicit exception.



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
