# 4-1 阶段入口

## FAST 阶段总控（4-1，默认）

将本文件作为**整个 04-catalog 阶段**的单一入口：① 按本文完成全量范围内商品/SKU/Variant/媒体/分类成员抓取，保存 expectedHandles、快照和失败清单；② **自动按 `04-catalog/4-2-商品CSV数据清洗.md`** 校验并生成不覆盖原始数据的 CSV，逐项对账，禁止静默漏商品/编造数据；③ 复核 `01-project/1-2-建站授权准备.md`，**自动按 `04-catalog/4-3-导入商品.md`** 做 dry-run、样本验证、已授权全量幂等导入与远端完整 Handle/Variant/媒体读回，保护既有商品状态，Online Store publication 另按明确授权与读回规则处理；④ **自动按 `04-catalog/4-4-创建分类.md`** 配置/核验 Collections、Menus、真实分类成员和必要筛选，保存 missing/extra；⑤ 仅在 CSV、权限、导入错误或分类对账异常时单独调用 4-2/4-3/4-4 处理失败项。**只有 CSV 或少数样本、未真实导入与对账时不得标记 4-1 通过。**

执行时读取 `EXECUTION_RULES.md` 和相关子 Prompt，**在同一轮执行所有必要工作并验证结果**，而不是只给计划、等下一条编号；记录当前项目 `PROJECT_MEMORY.md`、状态、证据和最小恢复步骤。子 Prompt 用于细节操作和定向排障，FAST 不等于跳过必要验收。

---


分析 {{REFERENCE_SITE_URL}} 的 sitemap、collection、公开商品数据接口和商品页，获取所有可公开访问的商品与 SKU，并生成可导入 Shopify 的 CSV。

先读取 `EXECUTION_RULES.md` 和当前项目记忆，核实源站与本任务范围。已完成的有效全量抓取可直接复用并核查时间、版本与清单；“继续”不代表重新抓取整个目录。

要求：

1. 从 sitemap、所有范围内导航分类和公开接口发现商品，保存发现清单与快照时间。逐页抓取直到可靠终止条件，记录分页游标/页码和失败 URL，不能只读第一页或首页推荐商品。各入口交叉检查遗漏，但不要无差别重爬重复页面。
2. 至少保留：Handle、Title、Body HTML、Vendor、Product Type、Tags、Status、Option 名和值、Variant SKU、Price、Compare At Price、Weight、Barcode、Image Src、Variant Image、SEO Title、SEO Description。
3. 同一商品的 variants 必须按 Shopify CSV 规范合并，图片和 variant 的对应关系要正确。
4. 输出原始抓取数据、规范化 JSON、最终 CSV 和审计报告，方便排错和重跑。
5. 不要臆造缺失的 SKU、库存、条码或价格；缺失字段明确记录。
6. 先生成少量样本并验证 CSV 结构，再执行全量导出。
7. 最终报告商品数、variant 数、SKU 数、图片数、重复项、缺失项和失败 URL。

全量完成要求：

- 保存完整预期 handle 集合、原始响应、规范化商品/variant 数据、分类成员快照、媒体角色映射和 CSV；源站颜色/尺码选项、可售状态与卡片/PDP 图片不能仅从文件名猜测。
- 样本通过后继续全量导出。用 `expected / exported / missing / failed / excluded` 逐项对账，失败项有限重试并保存恢复清单；没有消除或解释的缺口时，不报告全量完成。
- 记录明确排除项及依据，功能削减不自动意味着从原始抓取数据删除这些商品。Gift Card 等特殊商品保留真实类型和能力限制，不伪造成支持完整功能的普通商品。
- 原站当前目录与历史快照分开保存；只获取已有导入商品的详情，不能宣称已抓取原站所有商品。
- 抓取产物被 Git 忽略时，保留可重建命令、输入版本/哈希及路径，确保后续清洗、导入和迁移能找到数据。不要只在当前临时目录留下唯一副本。

直接实现并运行导出流程，不要只提供示例脚本。
