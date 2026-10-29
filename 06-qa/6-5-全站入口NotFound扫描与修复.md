# 6-5｜核心链接及变更差集 Not Found 检查（FAST / FULL）

> 默认 FAST 按 `STAGE_COMPLETION_CONTRACT.md` v2：优先使用 Shopify Menus、Pages、Collections、商品 handle 和本次 changed files 生成全部核心入口数据清单；可批量程序检查的 URL 直接验证，浏览器只抽检重要实际用户路径与异常点。未改变且已有可信检查的入口不重复打开。修复后只重测失败差集；不必为每个通过 URL 单独截图。用户明确 FULL/全站逐链接 UI 测试时再执行旧的深度流程。不能声称只抽样的入口都经过浏览器验证。



你现在的任务：在**当前明确指定的 Shopify 店铺和目标 Theme** 上，自动发现范围内所有用户可点击入口，集中定位 Not Found / 404 / 错误跳转 / 无效目标，按根因批量修复，再对受影响入口做差异复测。**不要只列建议，也不要用隐藏错误或跳转首页伪装成功。**先读 `EXECUTION_RULES.md`、`05-customization/5-2-功能削减.md`、`PROJECT_MEMORY.md`（若存在）。

## 边界与前置

- 先核对店铺 `*.myshopify.com`、当前 Shopify Partner/App 身份、实际权限、商品与 Online Store 发布情况；必须实际运行 `shopify theme list --store <store>.myshopify.com --json`，记录本次 Draft/Live Theme ID 和角色，不复用历史 ID。
- 默认检查和修改**获授权的 Draft Theme**，不因此发布 Live、关闭密码保护或覆盖 Live。只在本项目已有明确、仍有效的 Live 修改授权下修复 Live。修改 Shopify 共享的 Menus、Products、Collections、Pages、Policies 时先识别对 Live 的影响，并遵守既有权限与变更范围；不擅造政策正文。
- 在 Draft 预览中保留正确的 preview/theme 参数及密码会话；请求不能误落到当前 Live。Draft Preview 测试结果不可冒充公开 Live。需公开验证时使用独立无登录态会话，无法访问则记 `blocked`。
- 以 `05-customization/5-2-功能削减.md` 为范围源：Home、Header/桌面和移动菜单、Mega Menu、Logo、公告栏中实际存在的可点击入口、Hero/CTA、Collection/PLP、Product/PDP、卡片、筛选/排序/分页、搜索（适用时）、Cart、Footer 固定八条信息链接及本项目明确批准的其它页面。排除 Account/Login、Newsletter、Social、Gift Card、Blog/Article/FAQ 等默认已删除的功能和原站 Checkout/支付/订单确认页面；**已排除功能如仍显示入口，应按功能削减规范移除，而不是为其补建假页面**。Cart → Checkout 保留真实入口，由 `6-4` 单独验证到结账页即止，绝不提交订单或支付。

## 执行步骤：一次跑完

1. **建立入口清单**：读取 Theme Liquid/JSON/JS、Section Settings、Shopify Navigation Menus、Footer、实际产品/分类/Page/Policy 的 handle 与 publication；使用 1440px 桌面和 390px 移动端真实浏览器，遍历各范围内模板，展开 Mega Menu、移动抽屉、下拉菜单并抓取渲染后的链接和按钮跳转。包含隐藏在点击/悬停状态的二级、三级入口、动态生成的 URL 以及搜索结果/分页入口；去重但保留全部来源位置。对大量商品和分页，枚举**所有实际链接目标**并进行 URL/资源检查，浏览器交互则对每种模板和交互类型至少取代表样本；不得以只点首页几个链接代替全站扫描。
2. **逐项核验**：内部链接先检查 URL 解析及落点路径，带登录/预览态实际跟随必要跳转，结合 HTTP 状态、最终 URL、页面 DOM 内容、资源存在与发布状态判断。捕获 404/410、Shopify 的 `404 Not Found` 模板及软 404（即使 HTTP 200）、跳转到首页/密码页/错误商店、循环重定向、无内容页面、JS 路由失败、`href="#"`、空 URL、`javascript:void(0)` 和本应导航却没有行为的按钮。对于预期锚点，验证目标元素真实存在；对于外部站点的 403/反爬/CAPTCHA/超时，标记 `blocked` 而不是断言 404。记录测试环境差异、最终 URL 和响应证据。
3. **定位根因并集中分类**：把全部问题先汇总为机器可读清单，再按根因分组，包括错误 handle/path、已删除/未发布的 Product/Collection/Page/Policy、Menu 绑定错误、Theme 硬编码旧域名、动态链接拼接错误、Section 配置或分页/过滤失效、排除项残留、权限/密码拦截及外部站点失败。每个缺陷记录 `source page → visible label/element → source selector or code/config → target URL → final URL → status/evidence → root cause → proposed fix`；相同根因集中修一次。
4. **定向修复**：按真实资源和批准的页面范围修正 Menu、Section Settings、Liquid/JS 链接、handle、路径/资源关联；必要时在具有实际写入权限的范围内自主撰写并创建缺失的必需信息页/政策 Page、绑定真实资源并读回（参照 `3-pages-6`）；无需事前提供批准文案。默认移除范围外残留入口。禁止将所有死链统一指向首页、将 404 页面改成通用成功页、用空白占位或与实际商品数据不符的内容冒充完成，或用 CSS 隐藏范围内必需入口。缺失的信息/政策页应按前述规则自行起草为真实可访问的资源，并记录自动生成来源。先备份/记录原映射与差异，对共享数据变动先确认影响范围。主题变更默认仅推送到正确 Draft，并核实远端文件。
5. **完整复测**：在修复后的**相同 Theme/目标环境**重跑全量链接清单及桌面/移动端关键交互，核对每条原失败入口和受公共组件影响的其它页面；若引入新问题则继续修复再重跑，直至已授权可修范围内没有 P0/P1 死链，或明确列出真实 blockers。对实际公开 Live 的结果必须在已获授权发布/开放后**另做**无登录态核验。不得把部分抽样、HTTP 200、主题检查通过或按钮可见等同于全站通过。
6. **保留证据和恢复点**：当前项目写入 `agent-evidence/storefront-link-audit.json`（逐入口结果、来源、目标、最终 URL、状态、归因、修复前后）、`agent-evidence/storefront-link-audit.md`（汇总统计、P0/P1/P2、修改文件和资源、仍未解决项及环境边界），并更新 `PROJECT_MEMORY.md`。不得写入访问密码、token、Cookie、个人信息或敏感查询参数。

## 通过标准与交付

- 必需范围的链接入口逐项可追踪、实际目标存在且能正确打开；桌面菜单与移动菜单、八条 Footer 政策/信息链接、首页 CTA、集合卡片到商品、搜索/分页及 Cart 的相关导航没有 Not Found。
- 分开给出 `Draft: passed/failed/blocked/not_run` 和 `Live: passed/failed/blocked/not_run`；范围外入口标 `excluded`；外部/权限受阻不能算 passed。
- 汇报：检查总入口数、唯一 URL 数、原失败数、修复数、复测通过数、剩余失败与阻塞数、各问题的来源/最终落点及实际修复位置。若有 P0/P1 未修复，不得报告可交付。
- FAST 6-1 在同一轮自动执行本 6-5；单独 `运行6-5` 只执行此专项。结账入口专项 `6-4` 仍独立保留，不得被本扫描替代。
