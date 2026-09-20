# Shopify Prompts

用于 Shopify 店铺搭建、页面采集、主题复刻与精调、商品数据抓取、清洗与导入、分类配置、项目定制和交付验收的 Prompt 集合。

## 目录结构

```text
Shopify-Prompts/
├── 01-reference/
│   └── 页面爬取.md
├── 02-theme/
│   ├── 开始复刻.md
│   ├── 页面粗调.md
│   ├── 像素级精修.md
│   └── pages/
│       ├── 首页.md
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
│   └── 交付评估.md
├── tools/
│   ├── 切换CLI账户.md
│   └── 指定SunBrowser.md
├── README.md
└── .gitignore
```

## 使用方式

按当前任务进入对应目录，选择 Markdown 文件，将其中的 Prompt 复制到 AI 编程工具中，并替换文档中用 `{{...}}` 标记的变量。

页面复刻和商品数据处理可以并行进行，不需要等待其中一条流程完全结束后再开始另一条。

## 建议执行顺序

### 页面复刻

1. **采集页面参考**  
   使用 `01-reference/页面爬取.md` 分析目标网站的信息架构，自动选择具有代表性的首页、集合页、商品页、内容页等页面，并保存 Desktop / Mobile 截图、SingleFile、页面结构和交互参考。

2. **开始复刻主题**  
   使用 `02-theme/开始复刻.md`，根据已采集的 reference 和目标网站原页面创建 Shopify Theme，实现页面结构、视觉样式和主要交互。

3. **页面粗调**  
   使用 `02-theme/页面粗调.md` 修正明显的结构、布局、内容和交互差异。

4. **像素级精修**  
   使用 `02-theme/像素级精修.md` 对照 reference 和原网站继续校准字体、字号、间距、元素尺寸、图片比例、Sticky Header、响应式布局及页面交互，并通过浏览器实际验收。

5. **页面专项修复**  
   首页、商品详情页等特定页面出现问题时，使用 `02-theme/pages/` 下对应的专项 Prompt。

### 商品数据

6. **抓取商品数据**  
   使用 `03-catalog/爬取商品SKU.md` 分析 sitemap、Collection、公开接口和商品页，获取商品、Variants、SKU、价格、图片等数据，并输出原始数据、规范化数据、Shopify CSV 和审计结果。

7. **清洗 Shopify CSV**  
   使用 `03-catalog/商品CSV数据清洗.md` 检查 Handle、Variant、Option、SKU、图片、价格等字段，修复无法导入或商品分组错误的问题。

8. **导入商品**  
   使用 `03-catalog/导入商品.md` 将验证后的商品数据导入目标 Shopify 店铺。全量导入前先使用少量商品验证结构和结果。

9. **创建分类和导航**  
   使用 `03-catalog/创建分类.md` 根据商品属性和目标网站结构创建 Collections、Menus 和相关 Shopify 原生内容结构。

### 项目定制

10. **业务和视觉调整**  
    根据项目要求使用 `04-customization/` 下的 Prompt，例如折扣、功能削减、Cookie、LOGO 和粘性顶栏等。

### 交付验收

11. **交付评估**  
    使用 `05-qa/交付评估.md` 检查主题、商品、分类、价格、导航、响应式布局和关键交互，输出当前完成状态和仍需修复的问题。

## 工具说明

`tools/` 用于保存 Shopify CLI、浏览器和开发环境相关的操作说明，不属于主工作流。
