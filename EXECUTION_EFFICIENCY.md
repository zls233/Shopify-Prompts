# 执行效率与 Token 控制（2026-11-06）

> 适用于 0-1/1-1/2-1/3-1/4-1/5-1/6-1 及所有子 Prompt。**只优化执行路径，不减少任何现有范围、授权边界、完整高保真或验收要求。** 与高保真/安全规范冲突时，保留更严格的质量与权限要求。

## 一、模型负责判断，脚本负责重复劳动

- 模型：选择来源、定义范围/数据结构、编写和审查脚本、诊断无法自动处理的异常、决定修复和签收证据。
- 可复用脚本：批量页面/媒体采集、分页发现、CSV/JSON 规范化、对象差集、幂等导入、全量 API 读回、文件哈希与基本对账。
- 同类型操作第二次出现时优先抽象成项目脚本；避免大量 `python3 - <<'PY'` 临时块或对 TypeScript 源文件直接 `.replace()` 的补丁链。确需修改代码时使用明确上下文的补丁并检查语法和输入/输出契约。
- 单次命令执行不等于成功；脚本必须以退出码、机器可读 summary、证据路径、失败对象清单交接。

## 二、一次预检，最少上下文

- 阶段入口只读取当前 `PROJECT_MEMORY.md` 摘要、`agent-evidence/stage-state.json`、唯一阶段 Prompt 与其真正涉及的子章节；从仓库路由索引按需查全局规则，不在每一轮重新 `cat` 全文。
- 已知 Store、Draft ID、Git revision、来源版本、身份/权限或已验证对象若未发生变化，就复用同版本证据；仅在写入边界做必要的身份/权限复核。
- 原始 HTTP、DOM/HTML、截图、完整 API 响应、GraphQL introspection、媒体和日志保存为文件；stdout 优先输出 `{status,counts,failedIds,reportPath,nextAction}`。仅输出相关代码段/错误片段，限制行数；绝不回显凭据。
- 检查点保持精简（阶段、输入哈希、已核验 GID/Handle、真实差集、下一动作）。中断后从状态恢复，不回放全段旧聊天或对同一版本重复全量扫描。

## 三、关键前置门禁

- `2-1` 先用一个桌面+手机样本跑通**采集→媒体→离线可复核**，确认 HTML/CSS/字体/图片/视频证据链和路径正确，再跑范围内全部页面和状态；仍保留交互/动效逐项取证。
- `3-1` 在**任何正式批量商品写入前**确认来源发现清单和全部预期范围、各分页终止或阻断证据，核对 `expectedHandles` 与归一化 SKU/Variant/分类；先通过 `catalog:gate` 与代表性变体样本测试，再批量写入。部分数据绝不自动视为完整来源。
- `4-1` 首先按共享组件聚类差异，再补页面特例；同版本参考能复用则复用，每次局部修复后复测相关状态，最终完整 QA 不省略。
- Template 的通用脚本为**只读/本地审计工具**；不会自行授权数据写入、绕过源站权限、发布 Live 或覆盖已有证据。

## 四、有限重试、增量执行

- 网络/速率/权限/资源等错误分类处理；仅对可重试的 429/5xx/短暂断连做有限退避。403、验证码、无权限、额度保护不自动绕过或无限重试；故障变化才重试其他方案。
- 批任务记录 `inputHash`、稳定 ID、已完成项及失败详情，重复运行只补缺口。后台任务由脚本等待和摘要收集，不用模型反复 `ps/lsof/nettop` 轮询，除非确有卡死证据。
- 项目脚本先 dry-run/小样本，后全量幂等执行。全量验证通过后只重验变化对象和依赖对象；最终验收门禁仍必须满足原标准。

## 五、真实完成状态

- `captured` ≠ `verified`；`sourceManifestComplete` ≠ 独立证明源站全目录完整；`mutationSucceeded` ≠ 远端读回通过；`Theme Check passed` ≠ 浏览器/像素级完成。
- 必做项缺口必须为 `partial/blocked_core` 并可恢复。不得为了节省 Token 降低 2-1/4-1 视频、细动效、全 Section、移动响应式、像素对照、离线证据要求。
- 输出最终摘要：阶段/Store/版本、已核验数量、失败或阻断数量、准确证据路径、下一条恢复命令；不要生成大量含相同事实的重复报告。

## 六、可复用命令（项目使用最新版 Shopify-Template 后）

- `npm run reference:capture -- --plan references/reference-plan.json`：按已授权显式页面/状态清单采集原站，保存截图、HTML、DOM、媒体 URL 索引；不自动证明完整保真。
- `npm run reference:media -- --index references/capture-index.json`：源站资源限并发下载、哈希去重、失败记录；不绕过访问控制。
- `npm run catalog:gate -- --manifest data/catalog/source-coverage.json --products data/catalog/normalized.json`：导入前完整性前置门禁；无来源终止证据或有未覆盖 Handle 时失败。
- `npm run workflow:status`：读取简短项目 checkpoint，避免重复输出长文档。

上述脚本不代替 Shopify Admin 写入、远端读回、Offline replay、真实视频/动效、导航或 Draft QA。
