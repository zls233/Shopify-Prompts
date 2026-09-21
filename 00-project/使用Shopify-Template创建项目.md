# 使用 Shopify-Template 创建新项目

你现在需要基于 Shopify Template 模板创建或初始化一个新的 Shopify 项目。

模板仓库：

```text
https://github.com/zls233/Shopify-Template
```

这不是从零搭建 Shopify Framework，也不是立即实现完整店铺功能。目标是把已经在真实项目中反复出现的基础问题提前处理好，然后再进入页面复刻、商品数据和项目定制流程。

## 项目信息

执行前读取并确认以下变量：

- 项目名称：`{{PROJECT_NAME}}`
- Shopify 店铺：`{{SHOPIFY_STORE}}`
- 目标网站：`{{TARGET_SITE_URL}}`
- Draft Theme ID（如果已经存在）：`{{DRAFT_THEME_ID}}`

如果某个变量为空，不要使用示例值或猜测其他店铺。先检查当前工作区、Git remote 和已配置的环境文件；仍然无法确认时，停在该步骤并说明缺少什么。

## 一、先确认工作区，不要覆盖已有项目

1. 检查当前路径、Git 状态、远程地址和已有文件。
2. 如果工作区为空，使用 GitHub 的 `Shopify-Template` 作为起点，保留完整目录结构、`AGENTS.md`、README、Theme 骨架和检查脚本。
3. 如果工作区已经是一个 Shopify 项目，不要直接覆盖用户文件。先比较当前项目和模板，只补缺失的基础文件，并保留用户已有的主题、脚本、环境配置和未提交改动。
4. 不要把模板中的示例店铺、Theme ID、token、密码、商品 ID 或分类名称复制到新项目。
5. 初始化完成后，为新项目设置自己的 Git remote 和项目名称；不要把新项目的改动推回模板仓库。

模板本身是轻量起点，不要在初始化阶段新增：

- `sync-collections` 或通用资源同步框架
- Service/Repository 层
- 大型 Shopify SDK 封装
- 完整视觉回归平台
- CI/CD 工作流

只有同一个问题至少在两个真实项目中重复出现，并且解决方式稳定后，才考虑提升为模板能力。

## 二、初始化环境

1. 检查 Node.js、npm、Shopify CLI 是否可用。
2. 根据 `.env.example` 创建本地 `.env.local`。
3. 只在本地环境文件或系统凭据存储中配置：

```dotenv
SHOPIFY_STORE={{SHOPIFY_STORE}}
SHOPIFY_ADMIN_API_ACCESS_TOKEN=
SHOPIFY_API_VERSION=2025-10
SHOPIFY_STOREFRONT_URL=
SHOPIFY_THEME_ID={{DRAFT_THEME_ID}}
```

4. 不要在聊天、代码、Liquid、截图、审计报告、命令 URL 或 Git 历史中输出 token、密码或 client secret。
5. 安装模板依赖并运行基础检查：

```bash
npm install
npm run check
shopify theme check --path theme
```

如果当前项目没有 `theme/assets/theme.js` 或其他示例文件，以实际存在的文件为准，不要为了通过命令随意生成无关文件。

## 三、校验 Store 和 scopes

在任何 Shopify Admin API 写入之前，必须明确当前 store 和 App identity，并使用模板自带的轻量校验命令：

```bash
npm run verify:shopify -- \
  --store {{SHOPIFY_STORE}} \
  --require-scope read_products
```

根据当前任务追加最小 scopes。例如：

```text
Products 读取：read_products
Products 写入：write_products
Online Store 发布：read_publications、write_publications
Navigation：write_online_store_navigation
Metaobjects：read_metaobjects、write_metaobjects、read_metaobject_definitions、write_metaobject_definitions
Pages / Blogs / Articles：read_content、write_content
```

不要只根据 `shopify.app.toml` 判断 scope 已经生效。必须读取 `currentAppInstallation.accessScopes`。如果缺 scope：

1. 先确认目标 store 和 App identity 没有错。
2. 必要时执行 `shopify app build` 和 `shopify app deploy --allow-updates`。
3. 通过正常 OAuth 重新授权后再次校验。
4. 不要换用另一个项目的 token 绕过权限问题。

## 四、SunBrowser 是后台访问硬性要求

只要任务需要通过浏览器访问 Shopify Admin、登录、OAuth、密码页或其他后台 UI：

- 必须使用已经登录的 SunBrowser。
- 不要使用 IAB、普通 Chrome、其他浏览器 profile 或临时浏览器会话替代。
- 不要自动化登录、2FA、CAPTCHA 或绕过账号验证。
- 店铺密码只允许作为当前 SunBrowser 会话中的临时输入，不写入项目或长期保存。

如果 API 或 Shopify CLI 可以完成同一操作，优先使用 API/CLI；只有认证或 UI-only 操作才使用 SunBrowser。

## 五、Theme 开发边界

1. 默认只使用 Draft/Development Theme：

```bash
shopify theme dev \
  --store {{SHOPIFY_STORE}} \
  --path theme \
  --theme {{DRAFT_THEME_ID}}
```

2. 不要在没有明确授权时发布 Live Theme、覆盖线上主题、删除商品或删除 Collections。
3. Header、Mega Menu 和移动导航使用 Shopify Menu 的 `section.settings.menu.links` 与 `link.links`，不要在 Liquid/JavaScript 里维护第二套导航树。
4. Collection 页面使用真实 `collection.products`，保留 Shopify 原生排序、筛选、分页、价格、库存和商品链接。
5. 变体使用真实的 `product.variants`、`variant.options`、`variant.featured_media` 和 `selected_or_first_available_variant`，不要根据图片文件名猜测颜色或 Variant。
6. 可选 Metaobject 为空时使用 Shopify 原生对象作为安全回退。

## 六、最小交互验证

模板只提供最小 storefront smoke test。配置 `SHOPIFY_STOREFRONT_URL` 后运行：

```bash
npm run test:smoke
```

它只验证页面可以响应、文档可见且正文不为空，不替代项目级交互验收。

如果任务涉及参考网站复刻，后续再按需要使用 `01-reference/页面爬取.md`、`02-theme/开始复刻.md`、页面专项 Prompt 和 `05-qa/交付评估.md`。只有实际需要时才新增 `interaction-audit.md`、Playwright 断言或项目专用脚本，不要在初始化阶段提前建设完整视觉回归系统。

## 七、完成初始化后的报告

完成后输出：

- 项目路径和 Git remote
- 模板来源和初始化方式
- Store domain、App identity、已安装 scopes
- Draft Theme ID 与当前是否为 Live Theme
- 新增或保留的文件
- `npm run check`、Theme Check 和 smoke test 结果
- 尚未完成的登录、授权、环境配置或用户确认步骤

初始化完成后，项目才可以进入页面采集、Theme 复刻、商品数据导入、分类导航配置或定制开发阶段。

