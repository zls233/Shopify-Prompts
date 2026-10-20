# Shopify Prompts

## 统一展示与默认页面范围（新版）

- 全站 Announcement Bar：唯一英文文案 **`Free Shipping`**，无需复刻原站营销公告，且不因此擅改实际 Shopify 运费设置。
- Footer 固定两栏八条可用信息链接（**没有任何社媒或其它页脚栏目**）：

| CUSTOMER CARE | ORDER INFO |
|---|---|
| Privacy Policy | About Us |
| Return Policy | Contact Us |
| Shipping Policy | Terms Of Purchase |
| Service Policy | Terms Of Use |

- 这八个页面可以统一简洁实现，必须是正确的真实 Shopify Policy/Page 和获批准内容；不再 1:1 复刻源站复杂静态页。FAQ、Stores、Blog、Journal、Article、Warranty、Size Guide、独立 Editorial/Campaign 等默认 excluded，除非本项目明确要求或保留核心导航有已确认依赖。
- **此范围从 FAST 2-1 参考采集阶段就生效**，3-1 不补多余页面，5-1 负责清理，6-1 不以缺失超范围页面扣分。详细唯一规范见 [功能削减与范围清单](05-customization/02-%E5%8A%9F%E8%83%BD%E5%89%8A%E5%87%8F.md)。



> **默认六步 FAST：`1-1 → 2-1 → 4-1 → 3-1 → 5-1 → 6-1`。** 每个大阶段只需运行第一个 Prompt，自动完成必要子任务；仅遇问题时单独运行后续编号。

用于 Shopify 店铺搭建、页面采集、主题复刻与精调、商品数据抓取、清洗与导入、分类配置、项目定制和交付验收的 Prompt 集合。

所有阶段共用 [EXECUTION_RULES.md](EXECUTION_RULES.md)：按实际编号执行、复用当前项目与授权、全量数据对账、逐分类/逐页原站对照、共享数据与 Draft/Live 边界、分层验收和中断续做。2026-10-12 根据最近五个项目聊天更新，具体依据见 [项目经验记录](references/project-lessons-2026-10-12.md)。

## Checkout 范围与验收

从现在起，**不再复刻参考网站的结账、支付和订单确认页面**。Shopify 主题仍须完成商品规格选择、Add to Cart、Cart 以及 Cart 内真实有效的 Checkout 按钮；自有支付系统作为独立集成范围，不能用假页面模拟。发布并开放店铺后，用 `06-qa/04-购物车与结账入口测试.md` 在桌面/移动端真实加购并进入预定的结账界面，停在该界面，不支付、不下单。

## Shopify 账户与店铺授权规则

所有项目统一使用当前操作者自己的 Shopify Partner 账户进行 CLI、Admin 和开发操作。开始目标店铺工作前，必须由目标店铺通过 Partner / Collaborator access 授予该账户权限；Partner 登录成功不等于已获得店铺访问权。不得直接使用原店铺所有者账号，也不得用其代办授权。每条 CLI / API 命令都显式指定目标 `*.myshopify.com` 店铺，并分别记录 Partner 账户、店铺授权、Themes 权限与 Admin API 执行身份。主题开发统一使用已授权的 Partner 账户登录 Shopify CLI，不再要求 Theme Access token。

## 目录结构

```text
Shopify-Prompts/
├── 01-project/
│   ├── 01-创建项目.md
│   └── 02-建站授权准备.md
├── 02-reference/
│   └── 01-页面爬取.md
├── 03-theme/
│   ├── 01-开始复刻.md
│   ├── 02-页面粗调.md
│   ├── 03-像素级精修.md
│   ├── 04-原站素材复用.md
│   └── pages/
│       ├── 01-首页.md
│       ├── 02-顶栏与分类展开.md
│       ├── 03-底栏.md
│       ├── 04-商品列表.md
│       ├── 05-商品详情页.md
│       └── 06-页面补齐.md
├── 04-catalog/
│   ├── 01-爬取商品SKU.md
│   ├── 02-商品CSV数据清洗.md
│   ├── 03-导入商品.md
│   ├── 04-创建分类.md
│   └── shopify-catalog-collections/
├── 05-customization/
│   ├── 01-打折.md
│   ├── 02-功能削减.md
│   ├── 03-Cookie.md
│   └── 04-粘性顶栏.md
├── 06-qa/
│   ├── 01-交付评估.md
│   ├── 02-发布主题到Live.md
│   ├── 03-设置商家地址并开放店铺.md
│   ├── 04-购物车与结账入口测试.md
│   └── 05-全站入口NotFound扫描与修复.md
├── tools/
│   ├── 切换CLI账户.md
│   └── 指定SunBrowser.md
├── references/
│   └── project-lessons-2026-10-12.md
├── EXECUTION_RULES.md
├── PROMPT_ROUTING.md
├── AGENTS.md
├── README.md
└── .gitignore
```

## 使用方式

手动使用时，按当前任务进入对应目录，选择 Markdown 文件，将其中的 Prompt 复制到 AI 编程工具中，并替换文档中用 `{{...}}` 标记的变量。向代理发送编号指令时，代理直接读取并执行对应文件。

执行时同时提供本仓库规则和当前项目记忆。收到“继续”或“重试”先从已验证进度恢复；样本成功不等于全量完成，目录为空或页面验收阻塞不计入通过。公开 storefront、认证预览、主题文件和共享商品/内容数据分别判断，不能只凭上传成功宣称可交付。

默认 FAST 使用上方六个阶段总控，详细逐步执行仍可在 FULL 模式下选择；每一步必须完成对应真实验收。

## 编号指令路由

当用户发送“运行2-1”“运行3-pages-1”这类指令时，默认直接定位并执行本仓库中对应的 Prompt，不要求用户再次复制内容，也不把编号自行解释成其他流程。完整规则见 [PROMPT_ROUTING.md](PROMPT_ROUTING.md)。

常用映射：

- `运行2-1` -> `02-reference/01-页面爬取.md`
- `运行3-1` -> `03-theme/01-开始复刻.md`
- `运行3-pages-1` -> `03-theme/pages/01-首页.md`
- `运行4-1` -> `04-catalog/01-爬取商品SKU.md`
- `运行5-1` -> `05-customization/01-打折.md`
- `运行6-1` -> `06-qa/01-交付评估.md`
- `运行6-4` -> `06-qa/04-购物车与结账入口测试.md`
- `运行6-5` -> `06-qa/05-全站入口NotFound扫描与修复.md`

带有 `pages` 等明确后缀的完整编号优先于宽泛的阶段编号。找不到唯一匹配文件时必须报告候选，不得猜测或改跑其他 Prompt。

## 推荐执行顺序：FAST 六个阶段

在 Codex 运行前选择 **GPT-6.1-Sol + XHigh**（Prompt 本身不能切换模型）。默认 `EXECUTION_MODE=FAST`，阶段总控负责在同一轮完成必要处理、定向修复和验收，只有出现明确问题或用户特别要求才单独运行精修/清洗等子 Prompt。

| 顺序 | 只运行 | 阶段必须完成 | 失败时才单独运行 |
|---|---|---|---|
| 1 | `运行1-1` | 创建项目、确认店铺、环境与适用授权 | `1-2` |
| 2 | `运行2-1` | 参考页面、素材、截图、manifest 及自检 | 局部补抓 |
| 3 | `运行4-1` | **抓取 + 4-2 清洗 + 4-3 导入 + 4-4 分类**及读回对账 | `4-2/4-3/4-4` |
| 4 | `运行3-1` | 必需主题、素材、内容、响应式、加购/购物车及 Draft QA | `3-2/3-3/3-pages-*` |
| 5 | `运行5-1` | 折扣、5-2 功能削减、5-3 Cookie、按需粘性顶栏 | `5-2/5-3/5-4` |
| 6 | `运行6-1` | 交付评估/修复、**全站入口 Not Found 扫描与集中修复复测（6-5）**、**获授权才发布/开放**、公开后购物车 → Checkout 实测 | `6-2/6-3/6-4` |

**质量不能省：** 必须清洗并导入真实商品、逐项对账分类、修复明显视觉/功能缺陷，最终公开 Live 中真实加购并点进项目预期结账页面（不付款、不下单）。未获发布或开放授权时只能报告阻塞，不能擅自上线。

显式 `EXECUTION_MODE=FULL` 可按细分 Prompt 手动运行；单独 `运行4-2` 等仅处理该专项，不会重新运行整个阶段。所有阶段沿用 `EXECUTION_RULES.md` 和项目 `PROJECT_MEMORY.md` 的读回验收、幂等续做与授权边界。

## 传统逐步执行顺序（FULL 模式）

### 项目初始化

1. **使用 Shopify-Template 创建项目**
   使用 `01-project/01-创建项目.md`，从公开的 Shopify-Template 仓库初始化新项目，并在 GitHub 的 `Remix-Information` Organization 下创建或复用对应的 **Private 仓库**，核实组织归属和私有可见性后再推送。没有该组织的创建权限时暂停并请求授权，不要改建在个人账户或其他组织。随后确认 Draft Theme、Shopify Partners/CLI 登录、目标店铺 Themes 权限及基础检查状态。初始化阶段保持轻量，不提前建设通用同步框架或 CI/CD。

2. **建站资源授权准备**
   预计需要导入商品、创建分类、管理 Metaobjects 或创建 Pages / Blogs / Articles 时，先运行 `01-project/02-建站授权准备.md`。对店铺自有资源，模板的 `connect:shopify` 会复用 CLI 已保存的授权，缺失或权限不足时申请完整建站权限包并读回核对；App 自有资源使用所属项目 App 的授权。授权通过后继续原任务，不重复创建应用。

### 参考与商品

3. **采集页面参考**  
   使用 `02-reference/01-页面爬取.md` 分析目标网站的信息架构，自动选择具有代表性的首页、集合页、商品页、内容页等页面，并保存 Desktop / Mobile 截图、SingleFile、页面结构和交互参考。

4. **抓取商品数据**
   使用 `04-catalog/01-爬取商品SKU.md` 分析 sitemap、Collection、公开接口和商品页，获取商品、Variants、SKU、价格、图片等数据，并输出原始数据、规范化数据、Shopify CSV 和审计结果。

5. **清洗 Shopify CSV**
   使用 `04-catalog/02-商品CSV数据清洗.md` 检查 Handle、Variant、Option、SKU、图片、价格等字段，修复无法导入或商品分组错误的问题。

6. **导入商品**
   使用 `04-catalog/03-导入商品.md` 将验证后的商品数据导入目标 Shopify 店铺。全量导入前先使用少量商品验证结构和结果。

7. **创建分类和导航**
   使用 `04-catalog/04-创建分类.md` 根据商品属性和目标网站结构创建 Collections、Menus 和相关 Shopify 原生内容结构。

8. **补齐内容资源**
   默认只使用 `03-theme/pages/06-页面补齐.md` 补齐固定八个 Footer 信息链接缺失的 Shopify Policies/Pages；不再自动批量复刻 Blog/Article/Stores 等额外独立页面。需有真实内容与有效授权。

### 页面复刻

9. **整理并复用原站素材**
   使用 `03-theme/04-原站素材复用.md`，先盘点原站的视频、图片、文本、图案、SVG、图标和矢量资源，建立来源到 Shopify Section / 资源的映射，再实现和验证素材一致性。缺失素材要记录为阻塞，不要用生成内容或占位图填充。

10. **开始复刻主题**
   使用 `03-theme/01-开始复刻.md`，根据 reference 和已经导入的真实商品数据实现 Shopify Theme，先同步至 unpublished Draft Theme。

11. **页面粗调与像素级精修**
   依次使用 `03-theme/02-页面粗调.md` 和 `03-theme/03-像素级精修.md`，对照原站的桌面与移动端截图和交互逐轮验证。

12. **页面专项修复**
   视觉审计发现首页、商品列表、商品详情页、Footer 或顶栏与分类展开层的具体差异时，才使用 `03-theme/pages/` 下对应的专项 Prompt。分类导航的数据层问题先按 `04-catalog/04-创建分类.md` 处理，顶栏的视觉与交互问题使用 `03-theme/pages/02-顶栏与分类展开.md`。

### 项目定制

13. **业务和视觉调整**
    自动流程固定执行 `05-customization/01-打折.md`、`功能削减.md` 和 `Cookie.md`；不执行独立 `LOGO.md`。仅在原站确有粘性顶栏时执行 `粘性顶栏.md`。四折以原站当前售价为基准，须核验所有 Variant 的实际价格。

### 交付验收

14. **交付评估与修复**
    使用 `06-qa/01-交付评估.md` 检查 Draft Theme、商品、分类、价格、导航、响应式和核心购物交互。同时自动执行 `06-qa/05-全站入口NotFound扫描与修复.md`，对保留的全部页面入口集中扫描 404/软 404/错误跳转、按根因修复并完整复测。关键问题自动定向修复并重验；仍未通过则暂停。

15. **发布与公开验证**
    使用 `06-qa/02-发布主题到Live.md`，仅在项目绑定的专用店铺、Draft Theme 和商品发布状态全部核对后，发布明确指定的 Draft Theme。发布后重新读回 Live Theme 角色，并用无登录态请求验证公开页面。若仍存在 Private Mode，再按 `06-qa/03-设置商家地址并开放店铺.md` 处理；认证或外部限制无法自动完成时暂停。

16. **最终购物车 → Checkout 冒烟测试**
    运行 `06-qa/04-购物车与结账入口测试.md`，在已公开的 Live 店铺选择可售商品、加购、进入购物车、点击结账并确认真实结账界面加载。分别测试 Desktop/Mobile。未能真实进入时记录 `failed / blocked / not_run`，不将 Checkout 按钮存在当作成功。


## 工具说明

`tools/` 用于保存 Shopify CLI、浏览器和开发环境相关的操作说明，不属于主工作流。
