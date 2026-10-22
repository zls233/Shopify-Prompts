# 3-pages-3：固定八链接 Footer 专项

你现在负责当前 Shopify 项目页脚。**不再复刻源站 Footer**，唯一依据是 `05-customization/02-功能削减.md`；先读取 `EXECUTION_RULES.md` 与 `PROJECT_MEMORY.md` 并确认目标店铺、Theme 角色、访问权限。

## 固定页面结构

```text
CUSTOMER CARE                 ORDER INFO
Privacy Policy                About Us
Return Policy                 Contact Us
Shipping Policy               Terms Of Purchase
Service Policy                Terms Of Use
```

只保留这两栏、八条链接及必要排版背景，不创建其他分组、Footer Bottom Bar 或 Logo/社媒/Newsletter/支付徽章/地区选择器。图中红框只是标注，不是 CSS 边框。主题颜色优先遵循当前品牌设计，布局按图中的左右分组、行间距、标题层次和留白。桌面双列，手机响应式双列或纵向堆叠，不能出现水平溢出或空白占位。

## 执行流程

1. 检查现有 `sections/`、`snippets/`、`assets/`、`layout/theme.liquid`、当前 Footer Menu 与 Section Settings，列出所有多余栏目/社媒/订阅入口。清理对应渲染输出和 Footer 专属绑定，**不要只用 CSS 隐藏**；保留无关的 Shopify 数据和其他页面模块。
2. 对每个标签建立实际映射：`visible label → target URL → Page/Policy type → approved content source → status`。优先使用目标店铺正确的现有 Shopify Policies/Pages。法律与服务条款、Contact 信息绝不编造；`Terms Of Purchase`、`Terms Of Use`、`Service Policy` 不得随意全部指向同一个文档。
3. 目标资源缺失时按 `03-theme/pages/06-页面补齐.md` **仅处理固定八项中缺少的资源**。读取授权和真实内容来源；缺失批准正文则标记阻塞，不创建假政策骗过验收。
4. 复用现有 Footer Section 优先，按 Shopify Menu、正确对象 URL 或可配置 Section Settings 绑定真实链接，不写死旧店铺域名，不用 `#`、`javascript:void(0)`、空 `href`、404 或主页兜底。确保 hover、focus-visible、键盘交互及站外链接安全属性。
5. 默认仅更新正确 Draft Theme；远端读回后以 Desktop 1440px、Mobile 390px 真实浏览器检查 Home/PLP/PDP Footer，八项逐个点击核对内容、URL 和响应，不因按钮出现就称成功。
6. 更新 `PROJECT_MEMORY.md`；输出 store、Draft Theme ID、修改文件清单、移除项、八行 URL/资源/状态表、截图/日志证据及剩余 blockers。未经明确授权不发布或改写 Live。

**验收：** 恰好两组八个标签、没有社媒等额外元素、八个不同用途的真实有效目的资源、Desktop/Mobile 完整可用。若条款或联系信息缺失，不得把 `blocked` 写成 `passed`。
