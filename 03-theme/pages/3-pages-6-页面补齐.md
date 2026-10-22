# 3-pages-6：八个 Footer 信息页面补齐

本 Prompt 负责**当前允许范围内**缺失的信息页面/政策资源，绝不按参考站 sitemap 自动批量创建所有内容页。先读取 `EXECUTION_RULES.md`、`05-customization/5-2-功能削减.md` 和项目 `PROJECT_MEMORY.md`，确认 store、Partner/App 权限和真实资源状态。

## 默认只补齐八个 Footer 链接

| 固定标签 | 首选内容目标 |
|---|---|
| Privacy Policy | 目标 Shopify 店铺真实隐私政策或经批准的 Page |
| Return Policy | 真实退换政策/Refund Policy 或经批准 Page |
| Shipping Policy | 真实配送政策或经批准 Page |
| Service Policy | 真实服务条款或经批准 Page |
| About Us | 真实 About Page |
| Contact Us | 真实 Contact Page |
| Terms Of Purchase | 单独经核实的购买条款资源 |
| Terms Of Use | 单独经核实的使用条款资源 |

可以使用 Shopify 内置 `/policies/...` 或真实的 `/pages/...`，**不强制八个 Shopify Page 对象**；但八个标签必须分别核验目的地、文档内容、页面类型。不得把 Terms Of Purchase 和 Terms Of Use 混为一个未经核实的目标。

## 不再默认复刻的页面

FAQ、Warranty、Size Guide、Product Care、Stores、Store Locator、Blog、Journal、Article、Sustainability、Craftsmanship、Help Center、Editorial、Story、独立 Campaign/Landing Page、原站 Checkout/支付/订单确认页面，默认全部 `excluded`。旧 reference 或 sitemap 有这些路径**不构成新建授权**。项目用户明确需要额外路径或保留核心导航存在已确认依赖时，只处理被批准的精确路径，不重新开始全站 Blog/Article 批量复刻。既有资源不自动删除。

## 执行输入

```text
SHOPIFY_STORE={{SHOPIFY_STORE}}
PROJECT_PATH={{PROJECT_PATH}}
DRAFT_THEME_ID={{DRAFT_THEME_ID}}
TARGET_PATHS={{TARGET_PATHS_OPTIONAL}}
```

无 `TARGET_PATHS` 时，仅做八项中缺失部分。传入范围外路径时先核对该项目是否已明确新增该需求。目标店铺域名不能从示例、旧报告或浏览器标签猜测。

## 完整任务

1. 检查实际 Shopify Policies、Pages、现有 Footer Section/Menu，建八行映射 `label / resource ID / URL / content source / exists / verified`，只补缺失的、在范围内的真实资源。
2. 先执行或复核 `01-project/1-2-建站授权准备.md`；必须验证目标 store、实际执行身份、Pages/政策需要的权限，未授权不得写入。Theme 默认仅同步到 Draft；原生 Page/Policy 属共享店铺资源，可能立即影响 Live。
3. 政策、运费、退换、商家法律条款、公司历史、联系方式必须由目标商家真实配置或业务明确批准。不自行编造，也不复制源站法律文字冒充当前店铺政策。若没有可靠内容：记录具体缺项、`blocked` 与待批准负责人，不用空页或 lorem ipsum 伪装完成。
4. 需要创建时优先复用现有 Shopify Policy/标准 Page，稳定 Handle，避免重复；通过可用 CLI/Admin API 写入后读回实际资源。CLI/API 不支持的 Policy 写入不要虚构 mutation，使用已授权 Admin UI 或记为阻塞。
5. **不再逐页像素级复刻**八个信息页。采用统一、简单、可编辑、响应式的标题/正文 Shopify 模板，必要时复用一个标准 `page.json`；不用为 About/Contact 再做原站专属 Hero/Timeline/长图、为条款页制作多个独立 Theme 模板。
6. 按 `03-theme/pages/3-pages-3-底栏.md` 将八个真实 URL 绑定页脚；全部不能是 `#`、空链接、错误 404、首页兜底或文不对题。单独核对 Terms Of Purchase 与 Terms Of Use 的正文与链接。
7. 在 Admin 读回以及 Desktop 1440px / Mobile 390px 浏览器逐条验证正确 URL、页面标题、内容、公开访问和 Header/Footer 的正常显示。Draft 预览不等于 Live 公开访问，分开汇报。
8. 写入 `PROJECT_MEMORY.md`，记录八行最终结果、改动的真实 Page/Policy ID、源文本版本、权限、`verified / failed / blocked / not_run / excluded` 状态和恢复命令。

## 完成条件

八项均指向真实、合法来源且内容与标题相符的资源并经实际验证，才可报告八项全部完成。其它未获专项授权的独立内容页标记 `excluded`，不计入缺陷；没有获批准的政策正文或联系方式则报告真实 blocker。
