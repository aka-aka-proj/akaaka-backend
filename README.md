# akaaka-backend

這個 repository 的定位是承載 **Backend API / MCP service**。

## CI/CD 骨架

### PR CI (`.github/workflows/backend-pr-ci.yml`)
- 目標：在 Pull Request 上執行 backend 相關品質檢查。
- 行為：
  - 若存在可執行的 `scripts/ci/lint.sh`、`scripts/ci/test.sh`、`scripts/ci/build.sh`，依序執行。
  - 否則若有 `package.json`，執行 `npm` 的 `lint` / `test` / `build` scripts。
  - 若 repo 幾乎為空或沒有可執行 CI 契約，會 **fail-fast** 並給出明確錯誤訊息。

### Main CD (`.github/workflows/backend-main-cd.yml`)
- 目標：在 `main` 進行 **Supabase + Vercel** 取向的 API/MCP 部署流程。
- 順序固定為：**preflight → supabase（條件式）→ vercel deploy**。
- 行為：
  - `preflight`：檢查是否存在可部署 backend 內容，無內容時 fail-fast。
  - `supabase`：若 repo 含 `supabase/migrations` 或 `supabase/functions`，執行 migration/functions deploy；否則明確輸出 skip 訊息。
  - `deploy`：使用 Vercel CLI 部署 API/MCP（可直接接軌）。

## 建議後續接軌

1. 新增 `scripts/ci/` 與 `scripts/cd/` 實作腳本。
2. 在 GitHub Environments（例如 `production`）設定下列 secrets：
   - `VERCEL_TOKEN`
   - `VERCEL_ORG_ID`
   - `VERCEL_PROJECT_ID`
   - `SUPABASE_ACCESS_TOKEN`（有 Supabase 流程時）
   - `SUPABASE_PROJECT_REF`（有 Supabase 流程時）
   - `SUPABASE_DB_PASSWORD`（有 Supabase migration 時）
3. 讓 migration 保持可重入（idempotent），確保可重試部署流程。
