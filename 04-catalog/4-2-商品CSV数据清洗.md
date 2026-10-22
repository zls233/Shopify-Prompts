检查我提供的 Shopify 商品 CSV 是否可以直接导入。

读取当前抓取审计与完整 handle/variant 清单；CSV 结构通过只代表可进入样本导入，不代表商品已存在或全量导入完成。

重点验证：

- 表头是否符合当前 Shopify CSV 格式
- Handle 与多行 variant 分组
- Option1/2/3 Name 和 Value
- Variant SKU 缺失/重复及其源站证据，不为通过检查擅自改名或生成 SKU
- Price 与 Compare At Price 是否有效
- Published / Status 是否冲突
- Image Src、Image Position、Variant Image
- Product Category、Product Type、Tags
- 布尔值、空值、编码和换行
- 同一 Handle 下的公共字段是否正确重复或留空

先输出问题清单和受影响行数，然后生成一份修复后的 CSV。不要覆盖原文件。最后给出导入映射建议和小批量验证步骤。

对修复前后分别按 handle、variant 选项组合、SKU 和媒体映射对账，禁止因去重、空字段或失败行静默丢弃商品/规格。颜色名称大小写、`Color/Colour`、规格排序和售罄变体按源数据规范化，保留原值映射；不根据图片名称推断规格。报告 `expected / cleaned / missing / excluded`、原始/修复版路径和输入哈希，将所有未解决行列入后续导入的阻塞清单。
