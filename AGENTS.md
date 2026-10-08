# Shopify Prompt Routing

When the user says `运行2-1`, `运行3-pages-2`, or another numbered command,
resolve and execute the corresponding Markdown file in this repository. Read
`PROMPT_ROUTING.md` first. The number is a file selector, not a task summary.

Match the complete identifier, including `pages`, before using the phase
number. Require exactly one file match; if none or several match, report that
ambiguity instead of guessing. Read the selected Prompt, then execute it in
the current Shopify project using that project's `PROJECT_MEMORY.md` and the
Prompt's store, authorization, Draft/Live, and verification requirements.

For example, `4-1` is `04-catalog/01-爬取商品SKU.md`, while `3-pages-2` is
`03-theme/pages/02-顶栏与分类展开.md`. Do not substitute a previous audit or an
inferred workflow for the selected file.
