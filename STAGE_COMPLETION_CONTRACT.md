# FAST 阶段完整执行协议（2026-10-29）

此协议适用于 0-1 及 1-1/2-1/4-1/3-1/5-1/6-1 全部 FAST 入口，优先于旧文件中把“首批样本/计划/初次部署”当阶段完成的表述。

## 开始、执行与续跑

1. 先按 `PROMPT_ROUTING.md` 精确选择阶段，读取 `EXECUTION_RULES.md`、该入口及所有适用子 Prompt、项目 `AGENTS.md`、`PROJECT_MEMORY.md`。列出**所有适用**功能、数据、权限、Draft 浏览器验收、远端读回条件；保存到当前项目 `TASK_CHECKLIST.md`（不是通用 Prompts 仓库）。
2. 维护 `agent-evidence/stage-state.json`：`stage_id, canonical_prompt, contract_version, store, identity, theme_id, input_hash, expected, verified, remaining, blocked, batches, checkpoints, remote_evidence, resume_command, updated_at`，每批立即持久化，不在文件中储存 token、Cookie 或密码。
3. 样本成功之后在**同一次任务持续处理剩余差集**，逐批“执行 → 存断点 → 全量/差集远端读回 → 修复未通过项 → 复验”，全部适用检查项完成才结束阶段。不要向用户索要“继续 4-2/4-3/4-4”的下一条命令。
4. 新会话/上下文压缩/工具重连时先核对既有 checkpoint 和远端 Store/身份/Theme/输入哈希，仅重试真实未完成差集。已经成功的采集、商品创建、折扣及页面写入不得重做。
5. 大量 JSON、页面 DOM、截图和日志由本地脚本整理；返回给模型的仅是批次计数、异常样本、证据路径，避免重复输出长工具说明和 base64。对瞬时故障遵循 Retry-After、退避并最多同根因重试三次；对确定性错误、权限或反爬立即记录并停止对应支线，继续独立项。
6. `completed` 仅限范围内全部必需项 `verified` 或有证据 `excluded`；真实错误如实标记 `partial/blocked` 并提供失败列表与精确恢复命令。不能凭“写过脚本/上传过 Draft/只核验样本”宣称阶段完成。

## 默认商品状态与库存（目标店铺全部商品）

- 用户已明确指定：**目标 Shopify 店铺内新建及已有商品统一 `ACTIVE`，每个可跟踪库存的 Variant 在真实核验的履约 Location 上将 `available` 绝对库存设置为 `9999`**。这是新建站项目默认商品操作规则，4-1/4-3 负责执行，6-1 负责复验；覆盖已有 DRAFT 商品而不是仅本次新建商品。
- 以 Store + Product GID + InventoryItem GID + Location GID 作为幂等记录，先读取状态、tracking、地点和现有库存，再计算差集，使用当前 API 支持的绝对库存设置和必要的并发检查。不可做 `+9999` 增量，也不可对所有多地点库存各自填 9999。
- 非库存型/不可追踪商品若平台不支持设置，标注 `not_applicable` 及原因；其它错误必须保存准确 GID 和重试入口。保存并分页读回全店 Product 状态、各 Variant 库存级别的数量；不能只验样本或只信 mutation 返回。
- 源站原始 SKU/库存不受此默认值影响，原始采集字段仍按原样留证。新的 Shopify CSV `Status=active`；库存通过 Admin GraphQL + 正确 Location 读回确认。
- `ACTIVE` 不等于在线销售渠道已发布；不因本规则自行发布 Online Store、Live Theme、解除密码或改变支付。

## 固定八页内容模板

使用 `zls233/Shopify-Template` 的 `content/footer-pages/` 八个英文 HTML 模板。先读目标店铺真实品牌名、有效 HTTPS 网址、可用客服邮箱，再通过 `npm run footer:render` 生成八份 HTML/manifest；已存在的真实 Shopify Policies/Pages 优先，缺失的才以当前有效权限创建。Footer 必须恰好两栏八个不同用途的有效链接。Admin 读回及 Desktop/Mobile Draft 点击逐项验证；共享 Page/Policy 修改可能影响 Live。政策草稿须与真实运营和法律义务核对，技术完成不代表法律审批。

## 固定 FAST 顺序与交付

`1-1 → 2-1 → 4-1 → 3-1 → 5-1 → 6-1`。0-1 逐段运行；关键上游缺失不得伪装完成并进入依赖任务。每段输出 `verified/remaining/blocked` 计数、证据、当前真实 Store 状态、失败原因与唯一续跑命令。未获额外授权不得发布 Live、关闭密码保护、写第三方支付或创建订单。
