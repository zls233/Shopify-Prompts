# 导航结构、站内链接与源站外跳双重验收门禁（2026-11-05）

> 适用全流程 0-1/2-1/3-1/4-1/5-1/6-1 与菜单、Header、首页 CTA。与旧版 FAST 或外链 fallback 冲突时，本文件及 STAGE_COMPLETION_CONTRACT v3 优先。仅复刻获授权品牌内容，默认只写指定 Draft Theme；Menu/Page/Collection 为共享资源，先核对权限和 Live 影响。

## 一、两条独立的验收线

- **结构与交互保真**：忠实复刻原站实际桌面顶栏、Products/Mega Menu 多级分栏、分类切换、搜索/购物车、手机 Drawer 与对应 hover/click/focus/touch/scroll 状态。不能因为“链接能跳转”就把 Products 展开层简化为平铺一级导航。
- **链接和资源正确**：所有范围内的 Logo、顶栏/二三级菜单、Home Banner/CTA/分类卡片、PLP/PDP、搜索、Cart、Footer 链接必须指向目标店铺真实且用途正确的资源。单凭 HTTP 200、零个 404、链接个数不变或文字标题相同不算通过。
- 两项须分别有真实证据。任一未验证或未达到当前约定范围时，不得宣称 4-1/6-1 `completed_full`。

## 二、2-1：先采集结构和目的地

1. 分别采集源站 1440×900、390×844 和真实断点/窄桌面：默认、展开、hover/focus/touch、分类切换、关闭、滚动后的顶栏状态；保存截图、DOM、computed style、几何/媒体与交互证据。不能只截图顶栏关闭态就凭猜测实现。
2. 对每条范围内入口建立 `references/navigation-link-map.json` 或等价记录：`source_page`、`selector_or_menu_path`、`label`、`hierarchy`、`device_state`、`trigger`、`source_href`、`expected_kind`（collection/product/page/search/cart/anchor/approved_external）、`target_url`、`resource_id`、`publication`、`evidence`、`status`、`reason`。包括展开层内隐藏的二三级链接以及首页所有 CTA。
3. 源站 URL 与本站资源不可混为一谈。尚未确认的站内目标标为 `unresolved` 并在 3-1 解析；不因已截图/采集 HTML 就认为映射完成。

## 三、3-1/4-1：有真实目标才绑定

1. Catalog 导入后用 Shopify Admin 真实读回的 Product/Collection GID、成员、handle 与 Online Store publication 对齐导航映射。优先使用真实对象链接；Page/Policy、搜索、Cart 按正确站内路径和业务行为绑定。
2. **严禁兜底**到原官网 `https://source-site/...`、`//source-site/...`、`window.location`、拼接的原站 URL、`#`、空 href、`javascript:void(0)`、错误商店、自动跳首页或未经验证的 `/pages/<handle>`。尤其查 Header、Theme settings、JSON、Liquid、JS、Admin Menus、Metaobjects、生成器和重新同步后的输出。
3. 对必需且可创建的信息页，在授权范围内建立用途明确、内容实在的站内 Page 并读回；**不能**用简陋介绍页冒充商品集合、经销商定位、配件搜索等复杂功能。不虚构商品、授权经销商名单、法定身份、运费/售后承诺。缺数据、功能或权限则留下 `blocked` 和恢复步骤；范围外已批准删除项记 `excluded`，并删除入口。仅经明确批准且列出用途的外部服务/支付链接可白名单保留。
4. 修复**源头配置/菜单生成脚本**，不能只改当前 HTML。重复运行同步脚本仍不得写回原官网。导航结构与链接功能变更互相做回归，不能只解决一侧。

## 四、4-1/6-1：同一 Draft 的验收步骤

1. 静态扫描 Theme Liquid/JSON/JS/设置与 Admin Menu 的源站 hostname、绝对链接、JS/3xx 重定向；批量将全部映射与 Shopify resource/publication 对账。Template 的 `npm run audit:links -- --source https://source.example` 是**本地静态辅助**，不等于 Admin/浏览器验收。
2. 真实浏览器在 1440px Desktop、390px Mobile 和断点附近对比源站与 Draft：Header 和 Mega Menu 几何、顺序、列、打开/分类切换/关闭、键盘 Escape/focus、手机滚动锁、Home CTA、搜索、购物车。完整链接可批量 URL/资源验证；一级菜单、每种嵌套层级和风险差集做实际点击。
3. 每条检查记录源 href、预期 kind/URL、实际最终 hostname/URL、HTTP 状态、页面真实内容/关联商品及点击证据；软 404、错资源、外跳原官网、错误商店均算失败（即便 HTTP 200）。`target=_blank`、JS 跳转与 3xx 必须检查最终地址。允许外链按逐项白名单验证。
4. 修复后在**同一 Theme ID 的 Draft** 重新运行链接审计和桌面/手机头部交互/截图，查受公共组件影响的 Home/PLP/PDP；远端上传要 pull/readback。Theme Check、脚本成功、HTTP 200、截图或哈希任何单项都不够。
5. 输出 `agent-evidence/navigation-link-audit.json`（total/passed/failed/blocked/excluded、来源、期望与实际目标、状态、修复前后、证据），同步 `TASK_CHECKLIST.md`。未运行的检查记 `not_run`，有未闭合的当前范围差异记 `partial` 或真实 `blocked_core`。

## 五、效率与安全边界

只对相同源站版本采集一次完整参考；后续复用有效证据、按变更差集批量检查。不得因 FAST 省掉双门禁。每次写入前核对 Store、身份、Draft/Live 角色；不擅自改 Live、发布、密码、支付及真实商家承诺。
