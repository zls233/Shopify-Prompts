检查我提供的 Shopify 商品 CSV 是否可以直接导入。

重点验证：

- 表头是否符合当前 Shopify CSV 格式
- Handle 与多行 variant 分组
- Option1/2/3 Name 和 Value
- Variant SKU 是否唯一
- Price 与 Compare At Price 是否有效
- Published / Status 是否冲突
- Image Src、Image Position、Variant Image
- Product Category、Product Type、Tags
- 布尔值、空值、编码和换行
- 同一 Handle 下的公共字段是否正确重复或留空

先输出问题清单和受影响行数，然后生成一份修复后的 CSV。不要覆盖原文件。最后给出导入映射建议和小批量验证步骤。