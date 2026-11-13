# RepoFlow

**RepoFlow** 是面向 Claude Code 與 Codex 的通用 Git／Commit／PR 工作流程 Skill。其核心是**保護工作樹、逐項授權、單一目的 Commit、依最新主幹安全整合功能分支**。

## 核心能力

- 操作前檢查工作樹、目標分支及專案規範，避免覆蓋無關變更。
- Commit 訊息第一行使用 `Type | 說明` 格式，可加多行內文，並保留 `Co-Authored-By`。預設 Type：
  - `Feature`：程式變更，包含新功能、重構、測試、相依套件升級
  - `Fix`：修正錯誤
  - `Design`：設計文件
  - `Doc`：其他文件（README、說明、筆記）
  - `Sheet`：表格文件，例如遊戲技能表、升級表
  - `Tool`：工具腳本或專案專屬的小工具
- `status`、`diff`、`log`、`fetch` 等唯讀指令可直接執行；說「commit／提交」才會真的 Commit，說「準備／整理」只列出檔案與訊息草稿。
- 根據專案既有的分支策略執行 Branch、PR、合併與清理，不強制要求主幹一定叫 `dev`。
- 多個功能分支依序合併時，**每合併一個分支，下一個分支先 Rebase 到最新主幹，再重新驗證與合併**。
- 明確區分「載入 Skill」、「檢查 Git 狀態」與「授權修改歷史／遠端」。

## 你指定的整合習慣

以 `dev` 為主幹，A、B、C 都從原本的 `dev` 建立，合併順序為 A → B → C：

```text
dev <- A
git rebase B onto latest dev (包含 A)
dev <- B
git rebase C onto latest dev (包含 A、B)
dev <- C
```

每次 Rebase 後都需要重新檢查差異、解決衝突、執行必要驗證，並確保 PR／CI 符合儲存庫規範。完整細節見 [`sequential-rebase.md`](skills/repo-flow/references/sequential-rebase.md)。這是 **Rebase-before-merge 的分支整合方式**，不是要求所有合併都使用 Git 的 rebase merge 方法。

### 特別注意

Rebase 會重寫 Commit ID。若分支已經 Push，更新遠端可能需要 `git push --force-with-lease`；**RepoFlow 不會因為需要更新 PR 就自動強制推送**，必須另外取得授權並確認是可安全改寫的分支。也不會擅自合併、刪除分支或繞過 GitHub 的保護規則。

## Repository 結構

```text
RepoFlow/
├── README.md
├── CHANGELOG.md
└── skills/
    └── repo-flow/
        ├── SKILL.md
        └── references/
            └── sequential-rebase.md
```

一般 Git 任務只需讀取主要 `SKILL.md`；涉及多分支依序整合時才讀取參考文件，減少不必要的 Token 使用。

## 安裝

將 `skills/repo-flow/` **整個資料夾（含 `references/`）**複製到使用工具的 Skill 目錄：

| 工具 | 個人 Skill 目錄 | 專案 Skill 目錄 |
| --- | --- | --- |
| Claude Code | `~/.claude/skills/` | `.claude/skills/` |
| Codex | `~/.agents/skills/` | `.agents/skills/` |

安裝後應能找到 `~/.claude/skills/repo-flow/SKILL.md` 或 `~/.agents/skills/repo-flow/SKILL.md`。實際路徑依所使用工具的版本為準。

建立 GitHub Repository `RepoFlow` 時，將壓縮檔內 `RepoFlow/` 的**內容**放到 Repository 根目錄。

## 使用範例

- 「依 `Feature | 新增訂單查詢` 的格式準備 Commit，先列出將納入的檔案。」
- 「請依 A、B、C 順序合併回 dev，每次合併前都先 Rebase 到最新 dev 並完成驗證。」
- 「檢查目前分支能不能合併成 PR，先不要 Push 或 Merge。」

## 與其他 Skill 的分工

- **CodeCraft：** 負責程式實作及相關測試驗證。
- **RepoFlow：** 負責 Git 工作樹、Commit、分支、PR 與合併程序。
- **CodeReview：** 負責獨立程式碼審查。

可獨立安裝 RepoFlow，不需要同時安裝其他 Skill。專案有更明確的規範時，以該規範為準。

## 維護

- 版本：`v0.1.0`，尚未在 Claude Code 與 Codex 中實際功能測試。
- 建議測試：單一 Commit、已修改工作樹、多分支 A→B→C Rebase、PR 檢查失敗、Rebase 衝突、遠端分支需 force-with-lease、未授權禁止 Git 高影響操作。
- 未加入 `LICENSE`，公開授權方式由 Repository 擁有人決定。
