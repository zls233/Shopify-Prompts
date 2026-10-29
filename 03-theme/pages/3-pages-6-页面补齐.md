# 3-pages-6：八个 Footer 信息页面补齐

> **Footer 文案复用：** 八页英文内容统一来自 `Shopify-Template/content/footer-pages/`，使用 `npm run footer:render`（真实品牌名、店铺 HTTPS URL、真实客服邮箱）生成 HTML；优先复用已存在的 Shopify Policy/Page，对缺失项幂等创建并读回，固定两栏八链接在 Draft Desktop/Mobile 逐项验收。内容技术生成不代表已通过法律/运营审阅，Page/Policy 为共享资源。


本 Prompt 负责**当前允许范围内**缺失的信息页面/政策资源，绝不按参考站 sitemap 自动批量创建所有内容页。先读取 `EXECUTION_RULES.md`、`05-customization/5-2-功能削减.md` 和项目 `PROJECT_MEMORY.md`，确认 store、Partner/App 权限和真实资源状态。

## 默认只补齐八个 Footer 链接

| 固定标签 | 首选内容目标 |
|---|---|
| Privacy Policy | 已有 Shopify 隐私政策，缺少时采用平台模板/基于已验证配置撰写通用草稿 Page |
| Return Policy | 店铺现有政策优先；否则自主生成不承诺未经确认退换期限的通用正文 |
| Shipping Policy | 店铺真实配送配置优先；否则生成基于结账实际选项的通用配送说明 |
| Service Policy | 现有服务条款优先；否则自行起草通用服务说明 |
| About Us | 店铺真实品牌资料优先；缺失时自主撰写与店铺产品类别一致的原创品牌介绍 |
| Contact Us | 现有真实联系渠道优先；否则实现可提交的 Shopify 联系表单 |
| Terms Of Purchase | 现有购买条款优先；否则起草独立通用购物条款 |
| Terms Of Use | 现有使用条款优先；否则起草独立通用站点使用条款 |

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
3. **允许并要求自主补全**：先读取 Shopify 现有 Policies/Pages、Contact 表单、公开且可信的品牌介绍与必要店铺配置；目标站不存在相应页面、目标店铺缺少现成文本时，直接撰写适合该站的完整原创页面正文，不向用户反复索要文案、批准稿、简介或原站页面。政策类页面可采用 Shopify 模板和通用中性文字，避免臆造具体退货天数、运费、发货时效、公司资质、地址、真实客服邮箱或个人数据处理承诺；不要复制原站商家身份/法律文件并冒充目标店铺事实。将自动生成来源与待运营/法律审阅事项记到项目文档，**不因缺少预先批准文案而停止技术建站**。
4. 需要创建时优先复用现有 Shopify Policy/标准 Page，稳定 Handle，避免重复；通过可用 CLI/Admin API 写入后读回实际资源。CLI/API 不支持的 Policy 写入不要虚构 mutation，使用已授权 Admin UI 或记为阻塞。
5. **不再逐页像素级复刻**八个信息页。采用统一、简单、可编辑、响应式的标题/正文 Shopify 模板，必要时复用一个标准 `page.json`；不用为 About/Contact 再做原站专属 Hero/Timeline/长图、为条款页制作多个独立 Theme 模板。
6. 按 `03-theme/pages/3-pages-3-底栏.md` 将八个真实 URL 绑定页脚；全部不能是 `#`、空链接、错误 404、首页兜底或文不对题。单独核对 Terms Of Purchase 与 Terms Of Use 的正文与链接。
7. 在 Admin 读回以及 Desktop 1440px / Mobile 390px 浏览器逐条验证正确 URL、页面标题、内容、公开访问和 Header/Footer 的正常显示。Draft 预览不等于 Live 公开访问，分开汇报。
8. 写入 `PROJECT_MEMORY.md`，记录八行最终结果、改动的真实 Page/Policy ID、源文本版本、权限、`verified / failed / blocked / not_run / excluded` 状态和恢复命令。

## 完成条件

八项均指向**实际创建或复用**的正确 Page/Policy，正文完整、用途相符、Desktop/Mobile 实际验证成功，才可报告八项页面技术完成。自动起草的政策与品牌说明记录生成来源及待业务复核项，但不阻断页面开发的技术验收；真正的权限不足、API/UI 写入失败或页面不可访问仍按事实记为 `blocked`。其它独立内容页按范围 `excluded`。
