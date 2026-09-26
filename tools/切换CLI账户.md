请将 Shopify CLI 使用用户自己的 Shopify Partner 账户连接到以下目标店铺：

目标店铺：{{STORE_DOMAIN}}

要求：

1. 先检查当前 Shopify CLI 登录账号、已认证店铺和目标店铺访问权限。当前账号必须是当前操作者自己的 Shopify / Shopify Partner 账号。
2. 如果当前 CLI 账号不是当前操作者自己的 Partner 账号，执行 `shopify auth logout`，再运行 `shopify auth login`，选择当前操作者的账号。不得选择或保留原店铺所有者账号。
3. 如果用户的 Partner 账号尚未获得目标店铺权限，停止并要求目标店铺人员通过 Partner / Collaborator access 邀请并批准该账号；不要用原店铺账号代办。
4. 如果 CLI 生成设备验证码或授权 URL，必须在当前正在运行的 SunBrowser 中打开。
5. 不要使用 Chrome、Safari、系统默认浏览器或 Codex 内置浏览器。
6. 优先连接现有 SunBrowser 窗口和现有浏览器配置，不要启动新的无登录状态 SunBrowser 实例。
7. 如果自动打开浏览器无法确保使用 SunBrowser：
   - 截获 Shopify CLI 输出的授权 URL；
   - 连接名为 `SunBrowser`、Bundle ID 为 `com.adspower.SunBrowser` 的应用；
   - 在现有 SunBrowser 中新建标签页并打开授权 URL。
8. 等待我在 SunBrowser 中完成账号确认；看到“Successfully logged in to Shopify CLI”后继续。
9. 登录完成后必须运行以下命令验证，不能只根据浏览器成功页面判断。验证结果必须同时证明当前 Partner 账号和目标店铺一致：

   shopify store info --store {{STORE_DOMAIN}} --no-color
   shopify theme list --store {{STORE_DOMAIN}} --no-color

10. 最终报告：
   - Shopify CLI 当前账号（必须是当前操作者自己的 Partner 账号）；
   - 店铺完整域名；
   - 店铺 ID；
   - Live 主题名称和 ID；
   - 是否确认后续 CLI 命令能够访问目标店铺。

11. 本任务只切换用户自己的 Partner CLI 登录并验证目标店铺访问权限，不上传主题、不导入商品、不修改任何店铺数据。
