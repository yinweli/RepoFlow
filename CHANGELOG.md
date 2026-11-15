# Changelog

## Unreleased

- `Tool` 類型涵蓋開發流程設定（CI、任務執行器、`CLAUDE.md` 等 AI 指示檔）；只使用已定義的類型，不自創。
- 合併後清理分支時，若專案要求只剩主幹與目前分支但仍有其他分支，先列出並詢問使用者。
- 定義各 Commit Type 的用途；重構、測試、相依套件升級歸類為 `Feature`。
- Commit 訊息允許多行內文，並保留 `Co-Authored-By`。
- `git fetch` 等唯讀指令不需授權；區分「提交」與「準備／草擬」Commit。
- 減少每次使用的工具呼叫：合併狀態檢查、不重跑已完成的驗證、依任務大小調整回報。
- 移除參考文件中與 `SKILL.md` 重複的規則。

## v0.1.0 — 2026-11-13

- 建立通用 Git／Commit／PR Skill：`repo-flow`。
- 整合 Git 安全授權、工作樹保護與 `Type | 說明` Commit 格式。
- 納入使用者的「前一分支合併完成後，下一分支 Rebase 到最新整合主幹再合併」流程。
- 將多分支依序整合的詳細程序拆至按需載入的 `references/sequential-rebase.md`。
- 處理 Rebase 改寫歷史、遠端 `--force-with-lease`、PR／CI 與分支保護的風險。
- 提供繁體中文 README 與安裝說明。
