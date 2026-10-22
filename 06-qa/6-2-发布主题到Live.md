# Shopify 主题发布到 Live

请直接完成主题发布前检查、发布和发布后验证，不要只提供操作步骤。先读取 `EXECUTION_RULES.md`。用户在当前会话已明确要求发布本项目、本次范围的 Live 时，将仍有效的授权记入 `PUBLISH_AUTHORIZED=是`，不重复确认同一动作；历史版本已完成的发布记录不授权新版本，没有有效授权时只做只读检查。

本 Prompt 的“发布”专指把 unpublished Theme 切换为 Live。发布不等于购物链路已通过；在公开店铺可访问后须运行 `06-qa/6-4-购物车与结账入口测试.md`，真实测试商品加购、购物车和进入本项目配置的结账界面，不复刻源站结账页面、不付款、不下单。若用户只说“推送 Live”并且上下文是已 QA 的少量主题文件，按上下文区分操作类型：限定文件同步不执行 `theme publish`，也不把现有 Live Theme 切换为另一主题。明确授权文件同步时，重新读取目标店铺当前 Live ID，拉回指定文件核对差异，只对批准文件使用 `theme push --allow-live --nodelete` 和逐文件 `--only`；之后重新列出主题、拉回文件核对（自动格式化 JSON 按语义比较），并验证公开桌面/移动端受影响页面。保留仍未解决的商品、内容和 Cookie 跟踪问题，不能将“文件已上线”报告成“已可交付”。

## 输入

```text
SHOPIFY_STORE={{SHOPIFY_STORE}}
PROJECT_PATH={{PROJECT_PATH}}
TARGET_THEME_ID={{TARGET_THEME_ID}}
TARGET_THEME_NAME={{TARGET_THEME_NAME_OPTIONAL}}
PUBLISH_AUTHORIZED={{PUBLISH_AUTHORIZED}}
ADMIN_API_VERSION={{ADMIN_API_VERSION}}
PUBLIC_URL={{PUBLIC_URL_OPTIONAL}}
```

填写规则：

- `SHOPIFY_STORE` 必须是准确的 `*.myshopify.com` 域名，不得从旧报告、`.env` 或当前 Admin 标签推定。
- `TARGET_THEME_ID` 必须是已经完成 QA 的 unpublished Draft Theme ID。用户已明确指定待发布的开发主题时，按下方 Development 候选准备流程获得可发布 ID；其他情况下为空时不得根据名称猜测或直接选择第一个未发布主题。
- `PUBLISH_AUTHORIZED` 从用户明确填写值或本项目当前有效的发布指令解析；值为“否”或缺少授权证据时按只读模式处理。保留授权范围与依据，文件同步授权不扩大为切换主题授权。
- `PUBLIC_URL` 为空时，根据发布后读回的店铺域名确定，不要凭历史记录猜测。

## 一、读取项目规则并确认身份

1. 读取 `AGENTS.md`、`01-project/1-2-建站授权准备.md`、`06-qa/6-1-交付评估.md` 以及本项目的 Partners/CLI 授权记录。确认当前 Partner 账户已获得目标店铺 Themes 权限，并实际通过 CLI 列出主题。
2. 先确认 Shopify CLI / Admin 浏览器使用的是用户自己的 Shopify Partner 账户，并且该账户已获得 `{{SHOPIFY_STORE}}` 的 Partner / Collaborator 权限。不得使用原店铺所有者账号发布。
3. 区分两类身份：
   - Theme CLI 复用已登录的 Partner 账户和目标店铺 Themes 权限，用于主题列表、推送、发布和拉回；不传 Theme Access token。
   - Admin GraphQL 使用 `shopify store auth` / `shopify store execute` 或项目明确配置的 Admin App，只用于店铺、商品、出版状态和权限核验。
4. 不要打印、保存或提交 token、密码、授权 code、Cookie 或完整授权 URL。只报告凭据是否存在及验证结果。
5. 记录本次操作的目标店铺、Partner 账户、执行身份、API 版本、目标 Theme ID、当前时间和发布前后角色；不得记录敏感凭据。

## 二、发布前只读核验

先执行：

```bash
shopify theme list --store {{SHOPIFY_STORE}} --json
```

若本次已授权、已指定且已验收的目标角色是 `development`，先执行下方候选准备，再重新列出主题并对准确的 unpublished 候选执行本节检查；不能在转换前把 Development 当成合格 unpublished，或跳过候选复验。

必须从这次最新返回中确认：

- 返回店铺与 `{{SHOPIFY_STORE}}` 完全一致。
- 当前 `role: live` 的 Theme ID 和名称。
- `{{TARGET_THEME_ID}}` 存在、`processing: false`，且角色为 `unpublished`。
- 目标 Theme 不是当前 Live Theme。
- 目标 Theme 名称与 `{{TARGET_THEME_NAME}}`（如填写）一致；不一致时停止。

不要信任 README、历史审计、环境文件或上一次命令输出中的 Theme ID。不要把 `theme push`、Draft Preview、Theme Editor 预览或本地文件存在误认为已经发布。

在有 Admin GraphQL 能力时，另行读回并记录：

- `shop.myshopifyDomain`
- 当前应用的 Client ID / App 名称
- `currentAppInstallation.accessScopes`
- Online Store publication 状态
- 关键商品、Collection、Menu 和 Pages 是否已达到本项目交付要求

如果目标主题仍在处理、主题列表读回不一致、店铺身份不一致、授权身份不明或发布前 QA 有未解决的阻断问题，停止发布步骤，继续可独立完成的候选准备/修复和验证工作。

## 三、发布授权闸门

### Development 候选准备

如果用户已授权发布的已验收主题是 `development`，而 CLI 不允许直接发布该角色，不停止在这个限制上：从已验证的本地版本/远端开发主题及其设置准备一个具名的 unpublished 候选，显式指定同一 store 和 theme path，不覆盖当前 Live。

记录开发主题 ID、候选 ID、代码版本和设置来源；拉回候选文件核对 Liquid/CSS/JS/媒体哈希与 JSON 语义，验证资源引用、真实数据和 Desktop/Mobile 页面，完成后把准确候选 ID 设为 `TARGET_THEME_ID`。开发主题的旧 QA 不自动覆盖候选。候选不完整、原设置缺失或复验失败时不得发布；保留原 Development 和 Live。

### `PUBLISH_AUTHORIZED` 不是“准备发布”

只有同时满足以下条件才可继续：

1. `PUBLISH_AUTHORIZED=是`。
2. `TARGET_THEME_ID` 来自本次对目标店铺的最新主题列表。
3. 目标主题是明确的 unpublished Draft Theme。
4. 当前 Live Theme、目标 Draft Theme 和店铺域名已经向用户清楚说明。
5. 用户没有要求只发布文件、只同步 Draft 或保持 Live 不变。

如果任何一项不满足，执行到发布前检查即可，并报告“未发布”及缺少的条件。

## 四、发布执行

优先使用 Shopify CLI，并始终显式指定店铺和目标 Theme ID：

```bash
shopify theme publish \
  --store {{SHOPIFY_STORE}} \
  --theme {{TARGET_THEME_ID}} \
  --force
```

说明：

- `--force` 只代表已获得本 Prompt 的显式发布授权，不代表可以跳过前置核验。
- 不要使用没有 `--store` 或没有 `--theme` 的交互式命令。
- 不要使用 `--allow-live` 推送本地文件来代替主题发布；`theme push` 是文件同步，`theme publish` 才是切换 Live Theme。
- 不要在发布过程中删除其他 Theme、覆盖未指定 Theme、修改商品、修改菜单、关闭密码保护或接受付款/订阅协议。
- 如果命令返回认证失败、店铺无权限、Theme 不存在、主题正在处理或其他非瞬时错误，停止并记录原始错误类别；不要自动换账号、换店铺或重试不同 Theme。
- 对网络断开、TLS、429 或 5xx 仅做有限重试。重试前先重新读取主题列表，确认上一次发布是否已经成功，避免重复操作。

## 五、发布后读回

发布命令成功后，必须再次执行：

```bash
shopify theme list --store {{SHOPIFY_STORE}} --json
```

验收结果必须明确记录：

- `TARGET_THEME_ID` 的角色现在是 `live`。
- 原 Live Theme 的角色现在是 `unpublished`，除非 Shopify 返回了明确的其他状态。
- 目标主题 `processing` 已结束，或明确记录仍在处理中。
- 当前 Live Theme 的名称和 ID 与发布命令目标一致。
- 没有误发布其他 Theme。
- 本次候选相关文件与已验收版本一致；JSON 自动格式化采用语义比较。只上传成功或角色切换成功不能代替文件与页面核验。

如果角色没有切换，不能报告“已上线”；保留发布命令输出和读回结果，停止后续公开验收。

## 六、公开页面验证

发布后使用无登录态的 HTTP 客户端或独立浏览器访问 `{{PUBLIC_URL}}`。不要只用 Shopify Admin 预览、Theme Editor 或已登录后台窗口作为证据。

至少检查：

- 首页响应正常，最终 URL 未意外落到 `/password`（若店铺仍有 Private Mode，单独记录，不要在本任务擅自关闭）。
- 首页 HTML 是目标主题的实际公开输出，而不是旧主题或密码页。
- Header、Footer、代表性 Collection、PDP 和本次修改涉及的页面可以访问。
- Desktop 至少检查 `1440px`，Mobile 至少检查 `390x844`；关注横向溢出、主题渲染错误、资源加载失败和核心购买路径。
- 浏览器控制台中的 Shopify `shop.app` iframe、第三方遥测或 favicon 噪声与实际 Theme JavaScript 错误分开记录。

如果公开访问被密码、WAF、TLS、429、502 或认证拦截，分类记录为外部验证限制；不得把“发布命令成功”写成“公开页面已验证”。

## 七、审计与输出

如本次任务属于结构性发布或项目要求留档，保存不含凭据的机器可读审计，例如：

```json
{
  "store": "{{SHOPIFY_STORE}}",
  "action": "theme_publish",
  "targetThemeId": "{{TARGET_THEME_ID}}",
  "targetThemeName": "{{TARGET_THEME_NAME_OPTIONAL}}",
  "authorized": true,
  "prePublishLiveThemeId": "",
  "postPublishLiveThemeId": "",
  "themeProcessing": false,
  "publicVerification": "passed|blocked|not_run",
  "errors": [],
  "verifiedAt": ""
}
```

最终报告必须包含：

1. 目标店铺、Partner/CLI 主题访问身份、Admin API 身份和实际验证范围。
2. 发布前 Live Theme 与目标 Draft Theme 的 ID、名称和角色。
3. 实际执行的命令和发布后主题列表读回结果。
4. 是否完成无登录态公开页面验证，以及 Desktop / Mobile 范围。
5. Private Mode、认证、WAF、Theme Processing 或其他未完成项。
6. 明确区分：主题已切换 Live、公开 URL 可访问、商品/导航/结账达到交付质量。这三项不能合并成一个结论。
