请将 Shopify CLI 切换并登录到以下店铺：

目标店铺：{{STORE_DOMAIN}}

要求：

1. 先检查当前 Shopify CLI 登录账号、已认证店铺和目标店铺访问权限。
2. 如果当前 CLI 账号无权访问目标店铺，执行 `shopify auth logout`，再运行 `shopify auth login`。
3. 如果 CLI 生成设备验证码或授权 URL，必须在当前正在运行的 SunBrowser 中打开。
4. 不要使用 Chrome、Safari、系统默认浏览器或 Codex 内置浏览器。
5. 优先连接现有 SunBrowser 窗口和现有浏览器配置，不要启动新的无登录状态 SunBrowser 实例。
6. 如果自动打开浏览器无法确保使用 SunBrowser：
   - 截获 Shopify CLI 输出的授权 URL；
   - 连接名为 `SunBrowser`、Bundle ID 为 `com.adspower.SunBrowser` 的应用；
   - 在现有 SunBrowser 中新建标签页并打开授权 URL。
7. 等待我在 SunBrowser 中完成账号确认；看到“Successfully logged in to Shopify CLI”后继续。
8. 登录完成后必须运行以下命令验证，不能只根据浏览器成功页面判断：

   shopify store info --store {{STORE_DOMAIN}} --no-color
   shopify theme list --store {{STORE_DOMAIN}} --no-color

9. 最终报告：
   - Shopify CLI 当前账号；
   - 店铺完整域名；
   - 店铺 ID；
   - Live 主题名称和 ID；
   - 是否确认后续 CLI 命令能够访问目标店铺。

10. 本任务只切换 CLI 登录和验证访问权限，不上传主题、不导入商品、不修改任何店铺数据。