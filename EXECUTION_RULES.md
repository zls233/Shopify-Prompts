# Shopify Prompt 执行与完成规则

## Favicon 必需交接规则（所有新项目适用）
- FAST 2-1 采集源站真实 favicon，保存 `references/_shared/icons/favicon.<ext>`、`favicon.json`（真实来源、MIME、尺寸、哈希、状态）；FAST 3-1 让目标 Draft Theme 真正加载该图标，FAST 6-1 检查浏览器及远端实际呈现。
- 不混淆 favicon、Header Logo 与 Apple Touch Icon；不可把 HTML 错误响应、默认占位图或未经核实的图标标记成功。遇不兼容格式可制作兼容 PNG，保留原件。
- 先审查主题现有 favicon settings；Shopify `image_picker` 不能直接引用 Theme assets，如需设置则用合法店铺 Files 引用；另一方案是在主题 head 使用 `asset_url` 声明图标，禁止生成互相冲突的 link。
- 仅在已授权 Draft 内修改主题；店铺 Files 为共享资源，Live 仍需独立授权与验证。记录 `verified / failed / blocked / not_run`，并通过真实浏览器和 Network 核验。


## 新版公告栏、Footer 和页面收缩规则（全阶段适用）

**新版默认范围（高优先级）：**遵循 `05-customization/02-功能削减.md`：公告栏唯一 `Free Shipping on All Orders`，Footer 固定 CUSTOMER CARE（Privacy Policy / Return Policy / Shipping Policy / Service Policy）和 ORDER INFO（About Us / Contact Us / Terms Of Purchase / Terms Of Use）两栏八链接，**不复刻原站 Footer 且不保留任何社媒或多余 Footer 栏目**。这八个信息资源需真实内容和有效链接，但无需逐页原站视觉复刻。FAQ、Stores、Blog、Journal、Article、独立 Editorial/Campaign 等默认 `excluded`，只有项目明确确认的额外路径才处理。本文其它宽泛的“原站全部复制”、额外内容页面示例与旧 Footer 指令若冲突，以此规则为准。

- 2-1 仅采集真正会复刻的核心模板与八个政策/信息页必要来源映射，不再做无关静态页样本。
- 3-1 直接产出固定 `Free Shipping on All Orders` 公告栏、八链接 Footer 和真实可访问的简洁信息页；无须原站 Footer 或独立 Blog/FAQ/Stores 视觉复刻。
- 5-1 负责将此规则实际落地，6-1 必须逐项验收：桌面/移动公告栏唯一文案，Footer 8/8 标签和 URL 及正确内容，额外页面 excluded，原有真实 Cart→Checkout 测试仍保留。
- **免邮要求仅限主题公告栏文本**：全站展示 `Free Shipping on All Orders`；不操作、不核验真实 Shipping Profiles、Rates、Zones、Markets、配送国家及自有结账运费。FAST 3-1/5-1/6-1 只负责文案落地及 Desktop/Mobile 各模板呈现验收；不得据文案宣称实际运费免费。



本文件适用于本仓库的编号 Prompt、页面专项和附带技能。执行时同时读取选中的 Prompt、当前项目 `AGENTS.md` 与 `PROJECT_MEMORY.md`。用户当前明确的要求和范围优先；历史项目的店铺、主题、授权或数量不能复制成新项目默认值。

## AUTOPILOT 端到端总控

`运行0-1` 只从 `00-autopilot/01-全流程自动执行.md` 进入，自动依次运行 `1-1 → 2-1 → 4-1 → 3-1 → 5-1 → 6-1`，维护项目内持久化状态并核验后续跑；对子任务失败有限重试及定向修复，关键依赖/权限阻塞时停止依赖链。总控本身不新增发布 Live、关闭 Private Mode、修改支付或付款授权。详见该总控文件。

## Storefront Password 自动获取（初始化阶段）

- FAST 1-1 必须检查目标店铺的前台密码保护状态，优先尝试真正受支持的 Shopify CLI / Admin API 读取方式；仅能读取启用状态时，使用当前已授权 Shopify Admin 浏览器自动进入 Online Store → Preferences / Store access 读取现有密码，不要求用户手动复制。
- 将当前店铺域名和密码写入项目根目录被忽略的 `.env`：`SHOPIFY_STORE_DOMAIN`、`SHOPIFY_STOREFRONT_PASSWORD`；幂等更新这两个键，保留其他环境变量。不能把密码写入 Git、Prompt、报告、终端日志或 URL，也不能因此修改 Password protection 设置。
- 读取后用受保护前台验证结果；未启用记录 `not_required` 并清理陈旧密码，受权限或 UI 阻塞记录 `blocked`，继续不依赖密码的工作。Storefront Password 不代替 Partner/CLI 或 Admin API 认证。

## 默认 FAST / 可选 FULL 阶段模式

- **默认 `EXECUTION_MODE=FAST`**：大阶段只运行首个 Prompt，即 `运行1-1 → 运行2-1 → 运行4-1 → 运行3-1 → 运行5-1 → 运行6-1`。每一入口应在同一次任务读取实际子 Prompt、完成必要子任务和核验，不能只创建脚本、生成计划或要求用户依次手动调用 4-2/4-3。FAST 省人工触发，不省数据清洗、商品全量导入、分类、主题基本质量、定制和最终测试。
- 阶段覆盖：1-1 包含适用的 1-2 授权；2-1 完成参考采集；4-1 包含 4-2 清洗、4-3 导入、4-4 分类；3-1 包含素材/内容/响应式/浏览器自检及明显问题修复（3-2/3-3/pages 为按需专项）；5-1 包含 5-2 功能削减、5-3 Cookie、条件 5-4；6-1 包含评估与定向修复、**获独立授权才**执行 6-2/6-3、公开后执行 6-4 真实结账入口测试。
- 用户显式 `EXECUTION_MODE=FULL` 或点名子编号时，按对应单文件详细流程运行；单独 `运行4-2` 不启动整个 4-1。遇问题只对具体失败部分使用专项 Prompt、有限重试，不重复导入或打折。每阶段读取当前项目 `PROJECT_MEMORY.md` 和远端证据，逐项保存 `verified / failed / blocked / excluded / not_run`、目标 store/身份/Theme ID、数据差集和恢复入口。
- Theme 默认 Draft，共享商品/分类/价格必须有明确目标和资源权限；FAST/`运行6-1` **不授权**切换 Live、关闭 Private Mode、修改商家地址或支付配置。发布、公开访问、付款都是不同权限边界。
- 用户需在 Codex 运行前自行选用 **GPT-6.1-Sol XHigh**；Prompt 本身不能切换底层模型/推理等级，使用模型名称不代表工作已完成。

## 编号、项目与续做

- 按 `PROMPT_ROUTING.md` 解析完整编号，读取实际文件后执行。开工时简短记录“编号 → 文件 → 当前任务”；不沿用旧聊天中错误的编号释义。
- “继续”“重试推送”“承接某聊天”意味着从已验证的进度续做。核对本地文件、数据清单、远端读回和未完成项，不重跑已完成的导入、打折、安装或发布，不重复索取已明确授予的权限。
- 承接或迁移时核对品牌、源站、当前目录、Git remote、目标 store，以及环境文件、原始数据和被忽略的导入产物。旧路径、旧报告和 `Completed` 标记只能作为线索。
- 多项任务先列 Todo，逐项标记 `pending / in_progress / verified / blocked / excluded`。中断后保留剩余 handle/批次、有效产物、下一条恢复操作和授权范围；等待登录时继续独立的数据、代码或本地取证工作。

## 身份、浏览器与访问

- 默认复用用户自己的 Partners / Collaborator 访问和现有 CLI/Admin 授权；SunBrowser 仅在用户明确指定时使用。浏览器连接故障不代表店铺权限丢失，不因此切换账号或重建 App。
- 分别检查 Theme CLI 授权、Admin GraphQL App 身份与 scopes、浏览器连接、storefront 访问状态。已登录 Admin 不代表访客 storefront 无密码；访问密码也不提供资源写入权限。
- 可用目标 Draft 的认证预览或 Theme Editor 中的真实 storefront iframe 验收，核实 URL、store、Theme ID 和角色。这不能证明无登录态公开访问。
- 需要 storefront 密码时复用用户已提供或在已授权后台可读取的值，只写入被 Git 忽略的 `.env.local`；不回显或提交。没有密码也没有可用认证预览时，远端页面验收为 `blocked`，密码页不算正常页面或 404。
- 登录、2FA、CAPTCHA 等需要用户完成时只提出当前有效请求，不反复启动并失效多个设备授权。恢复后先读回原操作是否已成功，再续做。

## 全量数据的完成条件

- 样本只是映射验证。样本通过后，在任务授权范围内继续全量抓取、清洗、导入或发布；少量首页商品、第一页或几个样本不能报告为全量完成。
- 确定源站快照时间、发现方式、全部分页、期望 handle/variant 清单及明确排除项。用稳定标识逐项对账，数量相同也可能成员不同。
- 大批操作保存输入版本/哈希、store、执行身份、操作 ID、每批及每行结果。瞬时错误有限重试，重试前读回；只处理未解决项，权限、数据校验和 `userErrors` 不原样循环重试。
- 报告 `expected / verified / missing / failed / blocked / excluded` 及具体 handle。有未解决的范围内商品时为部分完成或阻塞。源站实时目录与历史导入快照分开报告；只验证已导入交集不能称全站匹配。
- 功能入口削减与数据导入范围分别判定，以用户当前任务和 `05-customization/02-功能削减.md` 为依据。不得静默漏导，也不得为凑数量把不支持的原生商品功能冒充成普通商品；Gift Card、库存、价格、SKU、规格有功能或证据缺口时列出限制。
- `ACTIVE`、Online Store publication、媒体处理完成、实际可售和页面可访问分别验证。主题 ZIP/上传不会创建商品、分类、菜单、Pages、Blogs 或 Articles。

## 分类必须从实际导航验收

- 从原站和目标站 Header/Mega Menu/Mobile 列出所有一级、二级、三级分类目的地及去重后的 Collection 清单，不只检查首页推荐集合。
- 对每个范围内分类采集完整源站成员和目标成员，保存 handle 映射，输出 `missing = source - target` 和 `extra = target - source`。没有规则证据时不只凭标题正则、一个 Product Type 或互斥分类推断成员。
- 区分普通品类、交叉分类、New Arrivals/Best Sellers/Sale 等专题。一个商品可属于多个真实分类；历史商品与当前源站下架商品单列差异，不擅自删除或下架。
- 用户只要求分类效果一致时以成员正确验收，排列不另设失败条件；用户要求顺序或页面完全对齐时再核对顺序。分类 UI 仍需单独对照。
- 自动分类与快照成员一致是两个结果。保留已有手动集合时说明未来新增商品尚不自动归类，不未经范围确认重建集合。
- 颜色/尺码等筛选先检查真实字段、`collection.filters` 及后台配置。需要安装/授权 Search & Discovery 时复用已授予的安装授权；缺少必要授权时继续独立的分类工作并记录依赖。

## 页面、图片与响应式一致性

- 建立页面/区块对照清单，按任务要求从首页逐页处理。Header/Footer 公共修复后复查首页、列表、PDP 和内容页；About、Journal、Article、Stores 不能只套通用图文模板。
- 先核对原文、真实字体文件/字重、SVG/Logo、媒体/CTA、区块数量/顺序与目标资源，再校准布局。缺失资源按所属数据流程补齐，不用假卡片、假评分或首页兜底链接掩盖。
- 图片记录原始宽高、显示框、比例、`object-fit`、`object-position`、`srcset/picture` 和实际素材。原站自然比例时等比显示，固定裁切框时复现其比例与裁切；不对所有图片套同一高度/比例或用 `object-fit: fill` 拉伸。
- Collection 首图/悬停图/滑动图不一定等于 PDP 第一张图，同一商品在不同集合也可能不同。以实际源站状态映射，购买字段继续使用 Shopify 原生对象。
- 同 viewport、高度、滚动位置、语言/币种和菜单/筛选状态，待字体和可见媒体加载后截图。对照首屏、中部和页尾，不用整页相似度代替逐区块检查。
- 检查源站关键断点前后、窄桌面/平板和窄手机；常用起点为 1440、1280、768、390、375、320px，按实际断点补充，不固定庞大尺寸矩阵。
- 记录差异、修改归属、前后证据和复验结果，解决范围内可修复的明显差异。原站自身溢出、动态内容和外部功能限制单列；无完整证据不承诺“零差异”。

## Draft、共享数据与 Live

- 实时读取主题列表，绑定明确 ID/角色。Theme 文件默认改 Draft/Development；用户已明确“发布 Live”或“更新 Live”且目标范围明确时，作为对应授权，不重复确认。
- Products、Collections、Menus、Pages、Blogs、Articles、Files、Metafields、Metaobjects 和筛选配置是共享店铺数据，不随 Draft 隔离。审计分别列出 Theme 文件与共享资源的改动及影响。
- Development 的验收不能直接算另一 unpublished Theme 的验收。若不能直接发布，按发布 Prompt 创建 unpublished 候选，核对文件/设置和资源绑定、复验后再发布准确 ID。
- 上传后从同一 store/Theme ID 拉回相关文件：Liquid/CSS/JS/二进制按哈希，自动格式化的 JSON 按语义比较。推送成功、文件一致、页面显示各记结果。
- Live 发布、文件更新、商品发布、公开访问是不同动作；“重试推送”不增加发布或资源修改授权。

## 结账页面范围与最终冒烟测试

- 不复刻原站的 Checkout/结账页面、支付输入/订单确认页面，不纳入 reference 采集、Theme 页面补齐、视觉 QA 或像素级修复；以 `05-customization/02-功能削减.md` 为统一范围依据。
- **购物车的 Checkout 按钮不可删除或改为假交互**；商品可售 Variant → Add to Cart → Cart → 项目真实配置的结账入口仍属于 Required。支付系统由项目另行集成，不能仅凭进入 Shopify 默认 Checkout 宣称自有支付已经接好。
- 已发布且公开可访问后，固定执行 `06-qa/04-购物车与结账入口测试.md`，在 Desktop/Mobile 真实浏览器中逐步加购、核对购物车、点击结账并确认进入项目允许的结账界面。只能验证到界面，不填写资料、不付款、不创建订单。
- 分别报告实际测试状态 `passed / failed / blocked / not_run`、店铺/Theme ID、Product/Variant、时间、脱敏的落点 URL 和浏览器证据；不可把 Theme 文件推送、按钮可见、Draft 预览或 HTTP 200 当作公开 Live 的结账入口验收。

## 交付与项目记忆

分别报告本地静态检查、远端 Theme 文件、商品/内容清单、全分类成员与导航、页面视觉、真实购物交互、Cart → Checkout 入口测试、Live 角色和无登录态公开访问。一层通过不能替代另一层；`blocked / not_run / excluded` 不计入通过数。空目录不能证明 PDP、筛选或加购正常，没有发现失败不等于已验证。

默认执行适用的静态检查、Admin/文件读回和定向浏览器观察。测试套件遵循当前项目的测试授权；用户明确要求逐分类测试时完成指定范围，不擅自扩大套件。

交付前检查干净 checkout 所需数据。脚本依赖被忽略的 `output/` 时，提供可重建命令、版本/哈希或明确的数据交接。换店铺须重新解析 App、publication、Theme ID 和资源映射，不复用硬编码目标。

结束时更新当前项目 `PROJECT_MEMORY.md`：路径/remote、源站快照、store/身份、Theme ID/角色、共享资源变化、验证结果、剩余问题和恢复入口；凭据只记录配置状态。
