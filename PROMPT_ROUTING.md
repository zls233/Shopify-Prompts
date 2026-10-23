# Prompt Command Routing

When a user says `运行...`, treat the identifier as a command to execute the
matching file in this repository's `Shopify-Prompts` collection. Do not invent
a new task from the number and do not reinterpret a catalog prompt as a QA or
release prompt.

## AUTOPILOT 总入口

`运行0-1` → `00-autopilot/0-1-全流程自动执行.md`；在同一任务自动串行完成现有六个 FAST 阶段，遵守各阶段验收、断点续跑和授权边界。

## FAST 阶段编号（默认）

| 入口 | 包含的必要工作 |
|---|---|
| `1-1` | 01 项目初始化 + 适用 1-2 授权 |
| `2-1` | 02 全阶段 reference 采集和校验 |
| `4-1` | 04 抓取、4-2 清洗、4-3 导入、4-4 分类 |
| `3-1` | 03 必要 Theme、素材、内容、Draft QA；精修按需 |
| `5-1` | 05 打折、5-2 功能削减、5-3 Cookie、条件 5-4 |
| `6-1` | 06 交付评估与修复，获独立授权才 6-2/6-3，6-5 全站 Not Found 扫描修复、6-6 双站逐分类商品对照修复，公开后 6-4 |

每个入口在**同一轮**完成必要子任务、真实核验与证据，不必逐个发送子编号；用户点名 `4-2` 等仍只执行该专项。FAST 不授予 Live 发布或关闭密码保护权限。参见 `EXECUTION_RULES.md`。

## Resolution

1. Normalize `运行` plus optional spaces, backticks, and punctuation.
2. Match the full identifier before applying any fallback:
   - `运行0-1` -> `00-autopilot/0-1-全流程自动执行.md`
   - `运行2-1` -> `02-reference/2-1-页面爬取.md`
   - `运行3-1` -> `03-theme/3-1-开始复刻.md`
   - `运行3-pages-1` -> `03-theme/pages/3-pages-1-首页.md`
   - `运行3-pages-2` -> `03-theme/pages/3-pages-2-顶栏与分类展开.md`
   - `运行3-pages-3` -> `03-theme/pages/3-pages-3-底栏.md`
   - `运行3-pages-4` -> `03-theme/pages/3-pages-4-商品列表.md`
   - `运行3-pages-5` -> `03-theme/pages/3-pages-5-商品详情页.md`
   - `运行3-pages-6` -> `03-theme/pages/3-pages-6-页面补齐.md`
   - `运行4-1` -> `04-catalog/4-1-爬取商品SKU.md`
   - `运行5-1` -> `05-customization/5-1-打折.md`
   - `运行6-1` -> `06-qa/6-1-交付评估.md`
   - `运行6-4` -> `06-qa/6-4-购物车与结账入口测试.md`
   - `运行6-5` -> `06-qa/6-5-全站入口NotFound扫描与修复.md`
   - `运行6-6` -> `06-qa/6-6-首页导航逐分类商品对照与修复.md`
3. For other identifiers, match the complete unpadded ID followed by `-` in the corresponding stage directory. For example, `3-pages-2` means `03-theme/pages/3-pages-2-*.md`. Never match only the trailing number.
4. Require exactly one matching Markdown file. If there is no match or more
   than one match, stop and report the candidates; never guess.

If the current Shopify project is outside this repository, locate the actual
`Shopify-Prompts` checkout first. If it is unavailable, ask for its location;
do not reconstruct the Prompt from memory or infer a task from the number.

## Execution

After resolving the file, read and execute its Prompt in the current project.
Read `EXECUTION_RULES.md` as the shared execution and completion contract.
Briefly record the exact identifier, selected path, and task before executing.
Resolve the target store, reference URL, theme roles, and credentials from the
current project memory and environment, not from an example in the Prompt.
Apply that Prompt's authorization, Draft/Live, dry-run, and verification rules.
The default action is execution, not explaining the Prompt or asking the user
to copy it into another tool.

An identifier with an explicit suffix takes precedence over the broad phase:
`3-pages-1` must resolve to the homepage page Prompt, while `3-1` resolves to
the general theme recreation Prompt. `4-1` always resolves to the catalog SKU
crawl Prompt unless the user names a different file explicitly.
