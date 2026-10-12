# Prompt Command Routing

When a user says `运行...`, treat the identifier as a command to execute the
matching file in this repository's `Shopify-Prompts` collection. Do not invent
a new task from the number and do not reinterpret a catalog prompt as a QA or
release prompt.

## Resolution

1. Normalize `运行` plus optional spaces, backticks, and punctuation.
2. Match the full identifier before applying any fallback:
   - `运行2-1` -> `02-reference/01-页面爬取.md`
   - `运行3-1` -> `03-theme/01-开始复刻.md`
   - `运行3-pages-1` -> `03-theme/pages/01-首页.md`
   - `运行3-pages-2` -> `03-theme/pages/02-顶栏与分类展开.md`
   - `运行3-pages-3` -> `03-theme/pages/03-底栏.md`
   - `运行3-pages-4` -> `03-theme/pages/04-商品列表.md`
   - `运行3-pages-5` -> `03-theme/pages/05-商品详情页.md`
   - `运行3-pages-6` -> `03-theme/pages/06-页面补齐.md`
   - `运行4-1` -> `04-catalog/01-爬取商品SKU.md`
   - `运行5-1` -> `05-customization/01-打折.md`
   - `运行6-1` -> `06-qa/01-交付评估.md`
   - `运行6-4` -> `06-qa/04-购物车与结账入口测试.md`
3. For any other identifier, resolve the numeric path directly using zero
   padding and the filename prefix. For example, `3-pages-2` means
   `03-theme/pages/02-*.md`.
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
