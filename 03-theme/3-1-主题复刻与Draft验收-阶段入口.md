# 3-1｜主题复刻与 Draft 验收（FAST 完整阶段）

> 默认 FAST 必须先阅读 `STAGE_COMPLETION_CONTRACT.md`；该文件定义核心完成/P0-P2/省 Token/断点和授权。此入口**仅使用下述精简流程**。旧版详尽的页面样式/检查维度已迁移到 `archive/full-guidance/`，只有用户显式 FULL、像素级验收或遇到未解决的专项问题时才按需读取对应段落；不在 FAST 首轮全量读取归档。实际项目的 `PROJECT_MEMORY.md` 和已保存检查点优先，目标 Store/Theme 必须以现时真实资料为准。

## 交付目标

用现有 2-1 Reference 和 4-1/4-4 的真实商品、Collections、Menus 实现 **Home / PLP / PDP / Cart** 及必要通用 Header、Footer、响应式和真实交互。优先复用已存在 Theme，不重建已有功能；只在授权的准确 Draft/Development Theme ID 写主题文件。商品/菜单/Page/Files 是可能影响 Live 的共享资源，另记变更与权限。

## 必做顺序

1. **读最少资料**：当前项目 Store/Draft/Live、源参考 manifest、商品与分类当前状态、已有主题及差异。阅读本入口与当下实际会实现的 `3-pages-1/2/3/4/5/6` 对应专项的小节；不默认先跑 3-2/3-3/3-4 的完整微调流程。需补素材时才按 3-4，出现严重视觉缺陷再按 3-2；3-3 像素级精修默认跳过，除非用户指定 FULL。
2. **结构与数据**：Home 正确展示关键模块；PLP 读取 Shopify `collection.products`，过滤/排序/分页可操作；PDP 使用真正商品 variants/图片/价格/可用性；Cart 读取实际购物车。Header 桌面 Mega Menu 与移动菜单必须可导航，Footer 只保留两组八个真实页面链接，无社媒/Newsletter/额外列。
3. **购买链路**：PDP/快速加购发起真实添加后自动显示 Cart Drawer、Cart Notification 或进入 Cart；显示正确规格/数量，失败给提示，移动端/键盘操作可用。保留 Cart → 当前项目批准 Checkout 入口；不复刻源站 Checkout/支付/订单完成页面。
4. **统一呈现**：顶栏恰好 `Free Shipping on All Orders` 一条展示文案，不能据此修改实际运费配置。对**真实优惠**商品卡依据 `price/compare_at_price` 计算百分比 Badge，统一外观且不对无折扣项虚构优惠。合理复用原站图片及主要 Logo/fav，避免错误裁切/明显错图。
5. **八页 Footer**：复用 `Shopify-Template/content/footer-pages/` 八份英文正文；读取真实品牌/店铺 HTTPS URL/客服邮箱并执行 `npm run footer:render`，优先保留已有合适 Policy/Page，只创建缺失真实资源；菜单全部绑定可点击 URL。正文用途不同、基础可读；待运营/法律核实的具体政策另记，不让技术完成卡在“待审批”。
6. **第三方支付 Logo 素材**：从 2-1 的真实主 Logo 证据生成独立可访问资产、写 `docs/payment-branding-handoff.md`（来源/路径/SHA/MIME/当前或待定 URL）。有授权 Shopify Files 优先；不要拿 fav、banner 或商品图假冒。支付参数（含只在真实集成中出现的 `_z9`）需另获实际接入权限；本阶段仅交接素材，不因第三方尚未接入而阻塞主题交付。
7. **一次验证闭环**：必要静态/Theme Check → 推送准确 Draft → 读回已变动的主题文件和本次变更的 Shopify 资源 → Desktop 1440 / Mobile 390 各走 Home→PLP→PDP→Add to Cart→Cart 代表性流程。对不同规格、折扣/无折扣商品做少量边界样本。只修影响使用的 P0/P1 和大面积视觉错位。

## 什么时候结束

**核心页面和核心链路可用，真实商品渲染正确、明显视觉问题已解决，前后台读回有证据，报告 `completed_core` 并结束本阶段。** 细小字体/间距、轮播动效、未要求的额外尺寸、非核心布局差异和非范围页面记 deferred，不必无限 Screenshot→微调→Screenshot。每模板最多两轮有进展的视觉调整；核心不可用时仍需修复或诚实报告 partial。

保留现有 `PROJECT_MEMORY.md`、`TASK_CHECKLIST.md` 和 stage-state，收尾只写一份短摘要。未授权发布 Live、关闭密码或修改第三方支付。需要 FULL / 1:1 像素级细节时才按需读取 `archive/full-guidance/theme-recreation-detailed.md` 并执行 3-2/3-3。
