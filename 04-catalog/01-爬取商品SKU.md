
分析 {{REFERENCE_SITE_URL}} 的 sitemap、collection、公开商品数据接口和商品页，获取所有可公开访问的商品与 SKU，并生成可导入 Shopify 的 CSV。

要求：

1. 处理分页、重复商品、重复 variant 和失效链接，不能只读取第一页。
2. 至少保留：Handle、Title、Body HTML、Vendor、Product Type、Tags、Status、Option 名和值、Variant SKU、Price、Compare At Price、Weight、Barcode、Image Src、Variant Image、SEO Title、SEO Description。
3. 同一商品的 variants 必须按 Shopify CSV 规范合并，图片和 variant 的对应关系要正确。
4. 输出原始抓取数据、规范化 JSON、最终 CSV 和审计报告，方便排错和重跑。
5. 不要臆造缺失的 SKU、库存、条码或价格；缺失字段明确记录。
6. 先生成少量样本并验证 CSV 结构，再执行全量导出。
7. 最终报告商品数、variant 数、SKU 数、图片数、重复项、缺失项和失败 URL。

直接实现并运行导出流程，不要只提供示例脚本。