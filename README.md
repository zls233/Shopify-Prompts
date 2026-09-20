# Shopify Prompts

用于 Shopify 店铺搭建、页面采集、主题复刻与精调、商品数据清洗与导入、SKU 导出、分类配置和促销展示的 Prompt 集合。

## 文件说明

- `页面爬取.md`：分析目标网站的信息架构，自主选择具有代表性的页面，并保存 Desktop / Mobile 截图、SingleFile、页面结构和交互参考。
- `开始复刻.md`：根据已采集的 reference 和目标网站原页面创建 Shopify Theme，实现页面结构、视觉样式和主要交互。
- `页面精调.md`：对照 reference 和原页面持续校准字体、字号、间距、元素尺寸、图片比例、Sticky Header、响应式布局和页面交互，并通过浏览器或 computer use 验收。
- `爬取商品SKU.md`：分析 sitemap、Collection、公开接口和商品页，获取商品、Variants、SKU、价格、图片等数据，并生成原始数据、规范化数据、Shopify CSV 和审计结果。
- `商品CSV数据清洗.md`：检查并修复 Shopify 商品 CSV 中的 Handle、Variant、Option、SKU、图片、价格等问题。
- `导入商品.md`：将校验后的商品数据导入目标 Shopify 店铺。
- `创建分类.md`：根据商品属性和目标网站结构配置 Collections、Menus 等 Shopify 原生内容结构。
- `打折.md`：配置商品价格、Compare At Price 和前端折扣展示。

## 使用方式

按实际任务选择对应 Markdown 文件，将其中的 Prompt 复制到 AI 编程工具中，并替换文档中用 `{{...}}` 标记的变量。

页面复刻和商品数据处理可以并行进行，不需要等待其中一条流程完全结束后再开始另一条。

## 建议执行顺序

### 页面复刻

1. **采集页面参考**  
   使用 `页面爬取.md` 分析目标网站的信息架构，自动选择具有代表性的首页、集合页、商品页、内容页等页面，并保存 Desktop / Mobile 截图、SingleFile、页面结构和交互参考。

2. **开始复刻主题**  
   使用 `开始复刻.md`，根据已采集的 reference 和目标网站原页面创建 Shopify Theme，实现页面结构、视觉样式和主要交互。

3. **页面精调**  
   使用 `页面精调.md` 对照 reference 和原网站继续校准，包括字体、字号、间距、元素尺寸、图片比例、Sticky Header、响应式布局及页面交互，并通过浏览器实际验收。

### 商品数据

4. **抓取商品数据**  
   使用 `爬取商品SKU.md` 分析 sitemap、Collection、公开接口和商品页，获取商品、Variants、SKU、价格、图片等数据，并输出原始数据、规范化数据、Shopify CSV 和审计结果。

5. **清洗 Shopify CSV**  
   使用 `商品CSV数据清洗.md` 检查 Handle、Variant、Option、SKU、图片、价格等字段，修复无法导入或商品分组错误的问题。

6. **导入商品**  
   使用 `导入商品.md` 将验证后的商品数据导入目标 Shopify 店铺。全量导入前应先使用少量商品验证结构和结果。

7. **创建分类和导航**  
   使用 `创建分类.md` 根据商品属性和目标网站结构创建 Collections、Menus 和相关 Shopify 原生内容结构。

### 整站调整

8. **配置促销**  
   使用 `打折.md` 调整商品价格、Compare At Price 和前端折扣展示。

9. **整站精调**  
   商品、分类和页面内容就位后，再次使用 `页面精调.md` 检查实际商品数据接入后的 Collection、PDP、导航、购物车和移动端表现，并修复最终差异。
