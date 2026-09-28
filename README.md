# Shopify Prompts

用于 Shopify 店铺搭建、页面采集、主题复刻与精调、商品数据抓取、清洗与导入、分类配置、项目定制和交付验收的 Prompt 集合。

## Shopify 账户与店铺授权规则

所有项目统一使用当前操作者自己的 Shopify Partner 账户进行 CLI、Admin 和开发操作。开始目标店铺工作前，必须由目标店铺通过 Partner / Collaborator access 授予该账户权限；Partner 登录成功不等于已获得店铺访问权。不得直接使用原店铺所有者账号，也不得用其代办授权。每条 CLI / API 命令都显式指定目标 `*.myshopify.com` 店铺，并分别记录 Partner 账户、店铺授权、Admin API 身份和 Theme Access 状态。

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
│   └── pages/
│       ├── 01-首页.md
│       ├── 02-底栏.md
│       ├── 03-商品列表.md
│       ├── 04-商品详情页.md
│       ├── 05-页面补齐.md
│       └── 06-顶栏与分类展开.md
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
│   └── 03-设置商家地址并开放店铺.md
├── tools/
│   ├── 切换CLI账户.md
│   └── 指定SunBrowser.md
├── README.md
└── .gitignore
```

## 使用方式

按当前任务进入对应目录，选择 Markdown 文件，将其中的 Prompt 复制到 AI 编程工具中，并替换文档中用 `{{...}}` 标记的变量。

手动使用时可以按任务选取 Prompt；`Shopify Agent` 的自动流程采用下方的串行顺序，并在每步验收后继续。

## 建议执行顺序

### 项目初始化

1. **使用 Shopify-Template 创建项目**
   使用 `01-project/01-创建项目.md`，从公开的 Shopify-Template 仓库初始化新项目，并确认 Draft Theme、Shopify Partners/CLI 授权和基础检查状态。初始化阶段保持轻量，不提前建设通用同步框架或 CI/CD。

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
   原站有内容路径时，使用 `03-theme/pages/05-页面补齐.md` 创建 Pages / Blogs / Articles；它同样以授权准备为前置步骤。

### 页面复刻

9. **开始复刻主题**
   使用 `03-theme/01-开始复刻.md`，根据 reference 和已经导入的真实商品数据实现 Shopify Theme，先同步至 unpublished Draft Theme。

10. **页面粗调与像素级精修**
   依次使用 `03-theme/02-页面粗调.md` 和 `03-theme/03-像素级精修.md`，对照原站的桌面与移动端截图和交互逐轮验证。

11. **页面专项修复**
   视觉审计发现首页、商品列表、商品详情页、Footer 或顶栏与分类展开层的具体差异时，才使用 `03-theme/pages/` 下对应的专项 Prompt。分类导航的数据层问题先按 `04-catalog/04-创建分类.md` 处理，顶栏的视觉与交互问题使用 `03-theme/pages/06-顶栏与分类展开.md`。

### 项目定制

12. **业务和视觉调整**  
    自动流程固定执行 `05-customization/01-打折.md`、`功能削减.md` 和 `Cookie.md`；不执行独立 `LOGO.md`。仅在原站确有粘性顶栏时执行 `粘性顶栏.md`。四折以原站当前售价为基准，须核验所有 Variant 的实际价格。

### 交付验收

13. **交付评估与修复**
    使用 `06-qa/01-交付评估.md` 检查 Draft Theme、商品、分类、价格、导航、响应式和核心购物交互。关键问题自动定向修复并重验；仍未通过则暂停。

14. **发布与公开验证**
    使用 `06-qa/02-发布主题到Live.md`，仅在项目绑定的专用店铺、Draft Theme 和商品发布状态全部核对后，发布明确指定的 Draft Theme。发布后重新读回 Live Theme 角色，并用无登录态请求验证公开页面。若仍存在 Private Mode，再按 `06-qa/03-设置商家地址并开放店铺.md` 处理；认证或外部限制无法自动完成时暂停。

## 工具说明

`tools/` 用于保存 Shopify CLI、浏览器和开发环境相关的操作说明，不属于主工作流。
