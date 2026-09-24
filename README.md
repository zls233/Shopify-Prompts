# Shopify Prompts

用于 Shopify 店铺搭建、页面采集、主题复刻与精调、商品数据抓取、清洗与导入、分类配置、项目定制和交付验收的 Prompt 集合。

## 目录结构

```text
Shopify-Prompts/
├── 00-project/
│   ├── 创建项目.md
│   └── 建站授权准备.md
├── 01-reference/
│   └── 页面爬取.md
├── 02-theme/
│   ├── 开始复刻.md
│   ├── 页面粗调.md
│   ├── 像素级精修.md
│   └── pages/
│       ├── 首页.md
│       ├── 商品列表.md
│       └── 商品详情页.md
├── 03-catalog/
│   ├── 爬取商品SKU.md
│   ├── 商品CSV数据清洗.md
│   ├── 导入商品.md
│   ├── 创建分类.md
│   └── shopify-catalog-collections/
├── 04-customization/
│   ├── 打折.md
│   ├── 功能削减.md
│   ├── Cookie.md
│   ├── LOGO.md
│   └── 粘性顶栏.md
├── 05-qa/
│   ├── 发布主题到Live.md
│   ├── 设置商家地址并开放店铺.md
│   └── 交付评估.md
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

0. **使用 Shopify-Template 创建项目**
   使用 `00-project/创建项目.md`，从公开的 Shopify-Template 仓库初始化新项目，并确认 Draft Theme、SunBrowser 和基础检查状态。初始化阶段保持轻量，不提前建设通用同步框架或 CI/CD。

0.5. **建站资源授权准备**
   预计需要导入商品、创建分类、管理 Metaobjects 或创建 Pages / Blogs / Articles 时，先运行 `00-project/建站授权准备.md`。对店铺自有资源，模板的 `connect:shopify` 会复用 CLI 已保存的授权，缺失或权限不足时申请完整建站权限包并读回核对；App 自有资源使用所属项目 App 的授权。授权通过后继续原任务，不重复创建应用。

### 参考与商品

1. **采集页面参考**  
   使用 `01-reference/页面爬取.md` 分析目标网站的信息架构，自动选择具有代表性的首页、集合页、商品页、内容页等页面，并保存 Desktop / Mobile 截图、SingleFile、页面结构和交互参考。

2. **抓取商品数据**
   使用 `03-catalog/爬取商品SKU.md` 分析 sitemap、Collection、公开接口和商品页，获取商品、Variants、SKU、价格、图片等数据，并输出原始数据、规范化数据、Shopify CSV 和审计结果。

3. **清洗 Shopify CSV**
   使用 `03-catalog/商品CSV数据清洗.md` 检查 Handle、Variant、Option、SKU、图片、价格等字段，修复无法导入或商品分组错误的问题。

4. **导入商品**
   使用 `03-catalog/导入商品.md` 将验证后的商品数据导入目标 Shopify 店铺。全量导入前先使用少量商品验证结构和结果。

5. **创建分类和导航**
   使用 `03-catalog/创建分类.md` 根据商品属性和目标网站结构创建 Collections、Menus 和相关 Shopify 原生内容结构。

6. **补齐内容资源**
   原站有内容路径时，使用 `02-theme/pages/页面补齐.md` 创建 Pages / Blogs / Articles；它同样以授权准备为前置步骤。

### 页面复刻

7. **开始复刻主题**
   使用 `02-theme/开始复刻.md`，根据 reference 和已经导入的真实商品数据实现 Shopify Theme，先同步至 unpublished Draft Theme。

8. **页面粗调与像素级精修**
   依次使用 `02-theme/页面粗调.md` 和 `02-theme/像素级精修.md`，对照原站的桌面与移动端截图和交互逐轮验证。

9. **页面专项修复**
   视觉审计发现首页、商品列表、商品详情页或 Footer 的具体差异时，才使用 `02-theme/pages/` 下对应的专项 Prompt。

### 项目定制

10. **业务和视觉调整**  
    自动流程固定执行 `04-customization/打折.md`、`功能削减.md` 和 `Cookie.md`；不执行独立 `LOGO.md`。仅在原站确有粘性顶栏时执行 `粘性顶栏.md`。四折以原站当前售价为基准，须核验所有 Variant 的实际价格。

### 交付验收

11. **交付评估与修复**
    使用 `05-qa/交付评估.md` 检查 Draft Theme、商品、分类、价格、导航、响应式和核心购物交互。关键问题自动定向修复并重验；仍未通过则暂停。

12. **发布与公开验证**
    使用 `05-qa/发布主题到Live.md`，仅在项目绑定的专用店铺、Draft Theme 和商品发布状态全部核对后，发布明确指定的 Draft Theme。发布后重新读回 Live Theme 角色，并用无登录态请求验证公开页面。若仍存在 Private Mode，再按 `05-qa/设置商家地址并开放店铺.md` 处理；认证或外部限制无法自动完成时暂停。

## 工具说明

`tools/` 用于保存 Shopify CLI、浏览器和开发环境相关的操作说明，不属于主工作流。
