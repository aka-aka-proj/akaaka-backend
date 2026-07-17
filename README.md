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
- 目標：在 `main` 進行 API/MCP 部署流程骨架。
- 順序固定為：**preflight → migrate → deploy**。
- 目前 `migrate` 與 `deploy` 為 placeholder，預留給後續接雲平台或 runtime。

## 建議後續接軌

1. 新增 `scripts/ci/` 與 `scripts/cd/` 實作腳本。
2. 在 GitHub Environments（例如 `production`）設定 secrets/vars。
3. 讓 migration 保持可重入（idempotent），再進行 deploy。
