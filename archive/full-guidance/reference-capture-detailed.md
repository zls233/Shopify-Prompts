# 2-1｜页面参考、视觉素材及交互采集（完整阶段入口）

> **执行前置（2026-10-29）：** 本文件为阶段总入口，请先读取 `STAGE_COMPLETION_CONTRACT.md`。阶段内适用子 Prompt 必须同一轮依次实际执行，按 `TASK_CHECKLIST.md` / `agent-evidence/stage-state.json` 维护完整范围、每批 checkpoint 和远端验收；样本通过后自动完成余量。不以计划/脚本/部分商品/主题 push 当作阶段结束，也不要求用户逐个下达子命令。只有硬阻断才以精确差集与恢复命令报告 partial/blocked。



## FAST 简化采集与完成门槛（覆盖本文过度严格的旧验收）

默认只保留实现当前范围必需的高代表性 Home/PLP/PDP/Cart 的桌面/移动参考、关键导航结构、真实 Logo/favicon 线索、重要图片/字体与主交互。先检查已存在有效素材，绝不重新从头下载。优先直接 DOM/HTML + 必要截图，不要求 SingleFile 离线像素级完美回放、每一屏位置的截图、所有动态状态或全站所有图片镜像。离线回放差异和非关键动态证据不足可记 `deferred`，不阻止 `completed_core`；核心页面不可读或关键导航缺失才属阻断。原站防护触发停止自动请求，保留可用结果；实际源站商品全目录抓取留给 4-1。

## 必做：品牌主 Logo 采集（用于第三方结账交接，FAST 2-1）

- 在源站的真实 Desktop/Mobile Header 识别品牌主标识（优先可复用的 SVG/PNG/WebP 原图；若页面仅以文字/内联 SVG 呈现，则保存真实 SVG 或可核验的字标构成证据）。记录实际显示颜色、透明底、宽高比，以及浅色/深色背景版本；对比导航菜单中的促销图、商品图，确保没有认错。
- 保存到 `references/_shared/branding/`，形成 `checkout-logo-source.json`：`brand`、`source_page`、`source_url_or_dom`、`file`、`mime`、`viewBox_or_dimensions`、`sha256`、`background_variant`、`status`。若只有纯文本字标，记录原站文字、字体和外观证据，交给 3-1 生成实际图片素材。
- **主 Logo、favicon、导航促销图是不同资源**。不能拿 favicon、`menu-*.jpg`、banner、商品首图或整张 Header 截图充作支付页 Logo。源站 Logo 无法可靠取得时如实记录 `blocked`，继续其它独立采集项，不能把未知图标标为已验证。
- 本步骤只采集品牌素材，不访问/复刻源站 Checkout，不修改任何支付系统。产物供 FAST 3-1 制作独立 Logo 文件，再交给未来的支付接入方。


## 必做：源站 Favicon 采集（FAST 2-1）
- 访问目标原站首页，优先从最终渲染 DOM 的 `link[rel~="icon"]`、`link[rel="shortcut icon"]`、SVG/PNG/ICO 声明中发现 favicon；同时检查 Web App Manifest 的 icons，并将 `/favicon.ico` 仅作为兜底候选。相对 URL 使用源站页面基址解析；同域不同尺寸选清晰且实际被源站使用的版本，不得误用 Header Logo、Apple Touch Icon 或其它品牌图片替代，除非证据表明它就是网站 favicon。
- 下载可公开获取的实际二进制文件到 `references/_shared/icons/favicon.<ext>`，保留原始文件并记录 `references/_shared/icons/favicon.json`：原网页、采集时间、原始 URL、rel/type/sizes、实际 MIME、像素尺寸或 SVG viewBox、文件 SHA-256、最终本地路径、选择依据、状态与失败原因。对于重定向、查询参数、SVG、ICO 多尺寸帧进行正确处理；禁止把 HTML 错误页保存成图标。SVG/ICO 若无法被目标主题直接使用，另外生成清晰的方形 PNG 兼容副本，保留原图，不要任意拉伸变形。
- 验证本地文件可解码、内容不是空文件、不是通用占位/默认图标；找不到可信源站 favicon 时标记 `blocked` 并给出真实诊断，不能自动生成或冒充成功。将 favicon 发现和验证结果写入 `manifest.json` / `site-reference-summary.md` 供 3-1 使用。
- 本项属于 FAST 2-1 必需采集，不要求用户另行运行子 Prompt；已存在可验证、来源一致的文件则复用，不做无意义重复下载。


**新版默认范围（高优先级）：**遵循 `05-customization/5-2-功能削减.md`：公告栏唯一 `Free Shipping`，Footer 固定 CUSTOMER CARE（Privacy Policy / Return Policy / Shipping Policy / Service Policy）和 ORDER INFO（About Us / Contact Us / Terms Of Purchase / Terms Of Use）两栏八链接，**不复刻原站 Footer 且不保留任何社媒或多余 Footer 栏目**。这八个信息资源需真实内容和有效链接，但无需逐页原站视觉复刻。FAQ、Stores、Blog、Journal、Article、独立 Editorial/Campaign 等默认 `excluded`，只有项目明确确认的额外路径才处理。本文其它宽泛的“原站全部复制”、额外内容页面示例与旧 Footer 指令若冲突，以此规则为准。


## FAST 阶段总控（2-1，默认）

将本文件作为**整个 02-reference 阶段**的单一入口：发现完整范围内导航/模板/路径，选择高代表性首页、PLP、PDP、Cart 外观及内容页面，完成 Desktop/Mobile 截图、SingleFile/HTML、资产与响应式取证、manifest、本地回放和缺失项补抓。向 4-1 交接完整商品/分类发现入口，向 3-1 交接页面/媒体/交互映射。复用已有有效快照；不因仅采集代表性页面就声称全量商品采集，且不对源站执行加购/结账。

执行时读取 `EXECUTION_RULES.md` 和相关子 Prompt，**在同一轮执行所有必要工作并验证结果**，而不是只给计划、等下一条编号；记录当前项目 `PROJECT_MEMORY.md`、状态、证据和最小恢复步骤。子 Prompt 用于细节操作和定向排障，FAST 不等于跳过必要验收。

---

你现在的任务是：根据我提供的一个网站 URL，自主浏览该网站，识别对网站复刻最有价值的页面，并保存必要的参考页面与结果。

我只会提供一个 URL，例如：

https://www.everlane.com

不要让我手动列页面，也不要一开始给我计划后等待确认。直接开始工作，自己判断并完成。

目标不是爬完整个网站，也不是保存所有页面，而是用最少数量的页面覆盖尽可能多的页面模板、布局、交互和视觉模式，最终产出一套适合后续由 Codex / AI 复刻 Shopify 网站的 reference。

**结账页面排除**：原站的 Checkout、支付页面、付款资料表单、订单确认页面不属于后续复刻范围；不进入、不截图、不归档这类页面，发现时标记 `excluded (checkout)`。保留商品页/购物车等允许的页面参考，本源站采集阶段继续禁止加购及结账操作；目标站的购物链路由 `06-qa/6-4-购物车与结账入口测试.md` 验证。

同时读取 `EXECUTION_RULES.md`。代表页面去重只减少重复归档，不减少导航、内容路径或商品分类清单：保存完整入口/目的地列表，商品及分类成员的全量抓取交给 `04-catalog/4-1-商品采集清洗导入与分类-阶段入口.md`，不能把少量参考页当作全目录。

每个参考页面保留从首屏到页尾的区块顺序和数量、文案/CTA、原始媒体 URL/宽高、实际显示框/裁切、字体及字重、Desktop/Mobile 素材。仅对当前范围内的核心 Home/PLP/PDP/Cart 与必要视觉变体取证；商品卡首图、悬停/滑动图与 PDP 图集单独记录，不默认是同一顺序。观察实际响应式断点并记录前后状态，待字体和可见懒加载图片完成后再截图。

==================================================
一、总体目标
==================================================

你的目标不是“抓尽可能多的页面”，而是：

- 模板覆盖率高
- 重复页面少
- 页面视觉参考完整
- Desktop / Mobile 都有参考
- 重要交互没有遗漏
- 页面归档可正常打开
- 对后续 Shopify Theme 复刻有直接帮助

默认目标：

最少必要的代表页面（常见 5～12 个，按实际模板决定）

如果 10 个页面已经足够覆盖主要模板，不要为了达到数量继续保存重复页面。

如果网站结构非常复杂，可以超过 25 个，但必须说明为什么。

==================================================
二、页面发现与筛选
==================================================

先从首页开始浏览整个网站的信息架构。

主动检查：

- 主导航
- mega menu
- mobile navigation
- footer
- Footer 只采集或核实固定八个信息链接需要的 URL/内容来源，不再收集原站社媒、App、声明与多余栏目以供复刻
- sitemap 入口
- collection / category
- product
- search
- 固定八个 Footer 信息链接的真实内容来源与目标资源映射（无需复杂页面视觉采集）
- 当前项目明确批准的额外内容模板（否则跳过）

至少判断以下页面类型是否存在：

Home
Product
Collection / Category
Search（适用时）
Cart/Cart Drawer 外观（不执行源站加购）
固定八个 Footer 信息链接的真实来源映射（无需逐页视觉复刻）
项目明确指定的其它页面（如有）

对结构明显重复的页面不要重复保存。

每种重要模板只选择最有代表性的 1～3 个页面。

优先选择：

- 信息最完整
- 交互最多
- 视觉结构最典型
- 能代表该模板主要特征
- Desktop / Mobile 差异明显
- 对后续复刻价值最高

不要因为“页面数量少”而保存大量结构相同的页面。

==================================================
三、Product 页面选择规则
==================================================

Product 页面不要随机选择。

优先选择能代表不同复杂度或不同 Product 模板的页面，例如：

- 多颜色
- 多尺寸
- 多个 variant
- 多图
- 视频
- accordion
- sticky add-to-cart
- 不同 gallery 布局
- 不同产品信息结构
- 特殊 merchandising
- cross-sell / recommendation
- bundle / kit
- product badges
- 不同价格展示形式
- sale / compare-at price
- 不同 variant selector

如果多个 Product 页面结构完全相同，只保存少量代表页面。

Product 页面选择时，优先比较其 DOM、布局、交互和视觉结构，而不是只看商品类别不同。

==================================================
四、Collection 页面选择规则
==================================================

Collection / Category 页面优先选择：

- 有筛选
- 有排序
- 不同商品卡片布局
- 不同 banner / hero
- 不同 merchandising section
- 不同分页方式
- infinite scroll
- pagination
- category tabs
- filter drawer
- sidebar filter
- collection landing content
- editorial blocks

如果多个 Collection 页面只是商品内容不同，但布局和交互相同，不重复保存。

==================================================
五、需要主动观察的交互
==================================================

浏览过程中主动检查并记录：

- header
- announcement bar（目标文案固定 Free Shipping，只采集位置/样式，不复制原站促销文案）
- mega menu
- mobile navigation
- dropdown
- accordion
- tabs
- carousel
- modal
- product gallery
- zoom
- variant selector
- filter
- sort
- sticky elements
- sticky add-to-cart
- newsletter popup
- cookie banner
- search overlay
- cart drawer
- store selector
- 固定两列 Footer 的桌面/手机布局（无额外栏目）
- mobile drawer

对于需要交互后才能看清结构的内容，可以安全地展开。

允许操作：

- 展开菜单
- 展开 accordion
- 切换 tab
- 切换图片
- 切换非购买性质的 variant
- 滚动页面
- 关闭 cookie banner
- 关闭 newsletter popup
- 切换 desktop / mobile viewport
- 打开 filter drawer
- 打开 mobile navigation
- 打开 mega menu
- 核对固定八链接 Footer 参考布局

不要执行：

- Add to cart
- Checkout
- 登录
- 注册
- 发送表单
- 订阅
- 支付
- 提交订单
- 任何不可逆操作

不绕过验证码，不绕过访问限制。

遇到：

- HTTP 429
- CAPTCHA
- 登录限制
- 地区限制
- 明显访问限制

跳过该页面，继续处理其他页面，并在最终结果中记录。

==================================================
六、默认归档方式：SingleFile CLI
==================================================

最终被选中的 reference 页面，必须优先使用：

gildas-lormeau/single-file-cli

保存为完整单文件 HTML。

不要把 MHTML 作为默认归档格式。

不要使用浏览器自带的普通 “Save Page” 代替 SingleFile CLI。

如果当前系统尚未安装 SingleFile CLI，先安装：

npm install -g single-file-cli

确认：

single-file --help

对于每个最终选中的页面，直接对原始线上 URL 执行 SingleFile CLI。

例如：

single-file \
  "https://www.example.com/products/example" \
  "references/product/example-product/page.html" \
  --browser-wait-delay 5000 \
  --browser-capture-max-time 180000 \
  --load-deferred-content true \
  --remove-unused-styles false \
  --remove-unused-fonts false \
  --filename-conflict-action overwrite

SingleFile CLI 只负责保存最终筛选出的代表页面。

不要让 SingleFile 无差别 crawl 整个网站。

页面发现、模板判断、去重和代表页面选择必须由你自己完成。

==================================================
七、禁止 MHTML / WebArchive 二次转换
==================================================

SingleFile CLI 只允许直接采集原始线上 URL。

禁止：

- 将已有 .mhtml 输入 SingleFile CLI 再转为 .html
- 将已有 .webarchive 输入 SingleFile CLI 再转为 .html
- 将本地历史归档重新包装后标记为 SingleFile 成功

例如下面这种方式是禁止的：

single-file references/page.mhtml references/page.html

因为这种方式无法恢复原 MHTML 已经缺失的：

- 字体
- CSS
- 图片
- JS 状态
- 动态资源
- 原始网络资源

如果原始 URL 当前返回 429、验证码或其他限制：

- 保留已有 reference
- 不进行二次包装
- 将归档方式标记为 fallback
- 记录失败原因
- 继续处理其他页面

manifest 中应使用类似：

{
  "archiveMethod": "mhtml-fallback",
  "archiveError": "Original URL currently returns HTTP 429"
}

==================================================
八、SingleFile 保存前的页面状态清理
==================================================

不要直接对刚打开的页面执行 SingleFile 保存。

保存前必须将页面整理到“正常浏览状态”。

必须处理：

1. 关闭 newsletter popup
2. 关闭 discount popup
3. 关闭 email capture popup
4. 关闭 cookie consent banner
5. 关闭 survey
6. 关闭 chat widget
7. 关闭非必要营销 overlay
8. 关闭随机 modal
9. 收起意外打开的 mega menu
10. 收起意外打开的 dropdown
11. 关闭非必要 drawer

默认 page.html 不应被随机 popup 或 cookie banner 遮挡。

如果 popup 无法通过普通交互关闭，但只是视觉遮挡，可以隐藏该 overlay，但必须记录。

不要通过删除页面主体内容来强行清理。

==================================================
九、保存前加载处理
==================================================

执行 SingleFile 保存之前：

1. 等待页面达到可交互状态
2. 等待字体加载完成
3. 等待首屏图片加载完成
4. 等待主要 JS hydration 完成
5. 至少完整滚动页面一次
6. 触发 lazy-loaded 内容
7. 等待主要图片加载
8. 回到合适的页面状态
9. 再执行 SingleFile CLI

如果页面始终无法达到 networkIdle，但已经正常渲染，可以继续保存。

networkIdle 超时本身不等于保存失败。

==================================================
十、重要交互状态单独保存
==================================================

不要让重要交互状态污染默认 page.html。

默认页面应保存为：

page.html

如果某个交互对复刻有价值，可以单独保存状态。

例如：

states/
  mega-menu.html
  mobile-nav.html
  filter-drawer.html
  newsletter-popup.html
  accordion-open.html
  sticky-add-to-cart.html

同时保存对应截图：

states/
  mega-menu.png
  mobile-nav.png
  filter-drawer.png
  newsletter-popup.png

只有确实有复刻价值的交互才单独保存。

不要机械地保存所有可能状态。

例如：

newsletter popup 如果对复刻无价值，只关闭它即可，不需要额外保存。

如果 popup 本身是品牌视觉的重要组成部分，可以额外保存。

==================================================
十一、每个页面必须保存的内容
==================================================

每个最终选中的页面至少保存：

- SingleFile 完整 HTML
- Desktop full-page screenshot
- Mobile full-page screenshot
- 原始 URL
- 页面类型
- 页面标题
- 关键页面结构说明
- 特殊交互说明
- archive method
- 保存状态
- 失败原因（如果有）

目录示例：

references/
  home/
    homepage/
      page.html
      desktop.png
      mobile.png
      metadata.json

  product/
    allpa-35l-travel-pack/
      page.html
      desktop.png
      mobile.png
      metadata.json
      states/
        gallery-secondary.png
        variant-selected.png

  collection/
    backpacks/
      page.html
      desktop.png
      mobile.png
      metadata.json
      states/
        filter-drawer.png

  about/
    company/
      page.html
      desktop.png
      mobile.png
      metadata.json

==================================================
十二、文件命名规则
==================================================

文件和目录名必须有意义。

避免：

page1
page2
test
capture
sample

优先使用：

product-womens-day-glove
collection-womens-shoes
about-company
returns-policy
stores-locator
blog-impact
campaign-fall-collection

使用稳定、可读、可根据 URL 或页面标题识别的 slug。

==================================================
十三、Desktop / Mobile 采集
==================================================

每个最终 reference 页面都必须观察 Desktop 和 Mobile。

Desktop 至少使用一个典型桌面 viewport，例如：

1440 × 1200

Mobile 至少使用：

390 × 844

截图优先使用 full-page screenshot。

同时记录 Desktop / Mobile 之间的明显差异：

- 导航结构
- menu 形式
- grid 列数
- hero 比例
- 图片排列
- spacing
- sticky 行为
- filter
- drawer
- accordion
- 字号
- header 高度
- section 间距
- 隐藏 / 显示内容
- footer 展开方式
- product gallery
- add-to-cart 区域

==================================================
十四、视觉与样式信息提取
==================================================

除了 SingleFile HTML，还应尽量记录实际视觉实现信息。

至少关注：

- font-family
- font-weight
- font-size
- line-height
- letter-spacing
- 主要文字颜色
- 背景颜色
- container 宽度
- section spacing
- grid gap
- border
- border-radius
- shadow
- breakpoint
- header 高度
- button 高度
- card spacing
- image ratio

不要依赖截图猜字体。

优先从运行时页面读取实际值。

==================================================
十五、字体信息
==================================================

尤其要记录字体。

不要假设 SingleFile 一定完美保存所有字体。

尽量提取：

- font-family
- font-weight
- font-style
- 加载状态
- woff / woff2 URL
- @font-face
- 实际使用页面

可以通过：

[...document.fonts].map(font => ({
  family: font.family,
  weight: font.weight,
  style: font.style,
  status: font.status
}))

以及：

performance
  .getEntriesByType('resource')
  .map(x => x.name)
  .filter(url => /\.(woff2?|ttf|otf)(\?|$)/i.test(url))

如果可以安全获取字体资源 URL，记录到 metadata。

如果实际字体文件可以公开访问，可以额外保存到：

references/_shared/fonts/

并生成：

references/_shared/fonts/fonts.json

例如：

{
  "family": "Example Sans",
  "weight": "400",
  "style": "normal",
  "source": "https://...",
  "file": "references/_shared/fonts/example-sans-400.woff2"
}

==================================================
十六、共享资源
==================================================

对于多个页面共同使用的资源，可以集中记录到：

references/_shared/

例如：

references/_shared/
  fonts/
  logos/
  icons/
  design-tokens.json
  navigation.json

不要重复保存大量完全相同的共享信息。

==================================================
十七、每页 metadata.json
==================================================

每个页面生成 metadata.json。

示例：

{
  "type": "product",
  "title": "Example Product",
  "url": "https://example.com/products/example",
  "archiveMethod": "single-file-cli",
  "archiveStatus": "success",
  "desktopViewport": "1440x1200",
  "mobileViewport": "390x844",
  "reason": "Representative PDP with multi-color variants, image gallery and accordion",
  "interactions": [
    "variant selector",
    "accordion",
    "sticky add-to-cart"
  ],
  "fonts": [
    {
      "family": "Example Sans",
      "weight": "400"
    }
  ],
  "notes": []
}

==================================================
十八、SingleFile 保存后验证
==================================================

不要把“文件已经生成”视为成功。

每个 SingleFile HTML 先保存到临时文件，不要直接覆盖已有的 page.html。重新打开临时文件验证通过后，再替换正式归档；失败时保留旧归档和旧截图。

逐页记录原站与本地回放的标题、最终 URL、主体文字量、主要区块、可见图片加载情况、样式表、实际字体、页面高度和遮挡状态。在相同 viewport 下，对比首屏及关键区块的原站截图与归档回放截图。Desktop 和 Mobile 都要检查；不能只依据 CLI 退出码、HTML 文件大小或统一的图片数量阈值判断成功。以该页面原站实际可见内容为准，纯文字页面不应因为没有图片而自动失败。

如果正文、关键图片、字体或布局明显少于原站，即使命令返回成功，也必须标为未通过。允许从原始线上 URL 重试一次；不能把残缺结果写成 success，也不能用旧归档重新包装成新结果。

检查：

- 字体是否正常
- 图片是否正常
- CSS 是否正常
- 页面主体是否存在
- 页面主要布局是否正常
- 主要文字是否存在
- 主要视觉内容是否存在
- 页面主体是否被 popup 遮挡
- 默认导航是否处于正常状态
- 页面是否明显变形

如果仍然失败，则进入 fallback。

在 metadata.json 和 manifest.json 中记录本次采集日期、归档状态、失败原因、是否沿用历史文件，以及原站/回放的关键验收数据。清单状态必须与实际保留的文件一致；历史文件不能冒充本次采集成功。

==================================================
十九、Fallback
==================================================

如果 SingleFile CLI 对某页面始终无法正确保存，允许使用：

rendered HTML / DOM
+
Desktop screenshot
+
Mobile screenshot
+
metadata.json

并在 manifest 中明确：

{
  "archiveMethod": "fallback",
  "archiveStatus": "partial",
  "archiveError": "..."
}

不要因为单个页面失败而停止整个任务。

如果原始 URL 当前已经 429，但已有旧截图或旧归档：

可以保留旧 reference，但必须标记为历史 fallback。

其他回放不完整的页面也一样：保留上一次可用的文件，标明其原采集日期和本次失败原因；若旧文件也未经充分验证，标记为 partial，不宣称完整成功。

==================================================
二十、重复模板判断
==================================================

对于没有保存的页面，要记录为什么没有保存。

例如：

- same PDP template as product A
- same collection layout as collection B
- same static page shell
- only text content differs
- duplicate mobile behavior
- same core header and in-scope content structure; standardized eight-link Footer instead of source Footer

不要只写“duplicate”。

要说明重复在哪里。

==================================================
二十一、站点总清单
==================================================

生成：

site-reference-summary.md

内容至少包括：

- 站点 URL
- 发现的主要页面模板
- 最终保存了哪些页面
- 每个页面为什么值得保留
- 哪些页面结构重复因此被跳过
- 主要导航结构
- header 结构
- 标准八链接 Footer 布局参考（不还原原站社媒/额外栏目）
- mega menu
- mobile navigation
- 主要视觉风格
- 主要字体
- spacing 特征
- 主要 UI pattern
- 主要 Product 模板
- 主要 Collection 模板
- 重要交互
- Desktop / Mobile 主要差异
- SingleFile 保存情况
- 保存失败页面
- fallback 页面
- 当前访问受限页面
- 429 / CAPTCHA 情况

==================================================
二十二、manifest.json
==================================================

生成：

manifest.json

示例：

{
  "site": "https://example.com",
  "archiveTool": "gildas-lormeau/single-file-cli",
  "pages": [
    {
      "type": "product",
      "title": "Example Product",
      "url": "https://example.com/products/example",
      "directory": "references/product/example-product",
      "archive": "references/product/example-product/page.html",
      "desktopScreenshot": "references/product/example-product/desktop.png",
      "mobileScreenshot": "references/product/example-product/mobile.png",
      "archiveMethod": "single-file-cli",
      "archiveStatus": "success",
      "reason": "Representative PDP with color, size, gallery and accordion"
    }
  ]
}

对于失败页面：

{
  "type": "product",
  "title": "Example Product",
  "url": "https://example.com/products/example",
  "archiveMethod": "fallback",
  "archiveStatus": "partial",
  "archiveError": "HTTP 429 while recapturing original URL"
}

==================================================
二十三、自检
==================================================

完成后自行检查：

1. 是否有首页
2. 是否至少有一个 Product
3. 是否至少有一个 Collection
4. 是否覆盖主要静态内容模板
5. 是否保存 header/footer 代表页面；Footer 只按固定八个标签记录实际政策来源/资源映射，无需采集原站完整栏目和社媒；不同条款来源仍需分别核实
6. 是否观察 mobile
7. 是否遗漏明显不同的模板
8. 是否保存了明显重复的页面
9. SingleFile HTML 是否正常打开
10. 字体是否正常
11. 图片是否正常
12. CSS 是否正常
13. 默认页面是否被 popup 遮挡
14. Desktop screenshot 是否存在
15. Mobile screenshot 是否存在
16. metadata.json 是否存在
17. manifest 是否与实际文件对应
18. fallback 是否正确标记
19. 429 页面是否记录
20. 是否错误地把 MHTML 二次转换成 SingleFile
21. 是否遗漏重要交互状态
22. 是否保存了太多无价值重复页面
23. 是否在替换旧归档前完成原站与本地回放的逐页对比
24. 是否把失败或未经验证的旧文件错误标成了本次 success

如果发现遗漏，继续补充，不要先询问我。

==================================================
二十四、最终汇报
==================================================

最终只需要向我汇报：

- 共发现多少种页面模板
- 实际保存多少个页面
- 保存了哪些页面
- 哪些模板被认为是重复的
- 哪些页面因为限制没有成功保存
- 哪些页面使用了 fallback
- 哪些交互状态被额外保存
- reference 文件保存在哪里
- site-reference-summary.md 在哪里
- manifest.json 在哪里

不要只给分析或建议。

不要一开始给我计划后等待确认。

直接使用浏览器 / Computer Use / Playwright / SingleFile CLI 实际完成：

浏览
→ 页面筛选
→ 去重
→ 页面状态清理
→ SingleFile 保存
→ Desktop / Mobile 截图
→ 本地回放验证
→ metadata
→ manifest
→ summary
→ 最终自检
