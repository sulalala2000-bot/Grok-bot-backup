# Grok Bot backup / Grok Bot 環境備份

Public, scrubbed snapshot of a Grok Bot setup (agent **總經理** + user skills).  
這是一份已去除個資與密鑰的 Grok Bot 環境公開備份（代理人「總經理」＋使用者 skills）。

**Repo:** https://github.com/sulalala2000-bot/Grok-bot-backup  
**Exported:** 2026-10-25 (Asia/Taipei)

---

## English

### What this is

A portable tree you can use to restore **skill definitions** and remember which **connectors** were installed. It is **not** a full VM image and does **not** contain login secrets, cookies, vault entries, or email contents.

### Contents

| Path | Contents |
|------|----------|
| `skills/` | All user skills from `workflows/*/SKILL.md` (22 skills). Many are thin pointers into Google Drive. |
| `skills/DRIVE_LIBRARY_NOTE.md` | Link to the shared Drive library. |
| `agent-profile/` | Scrubbed `profile.json` / `settings.json` for agent 總經理. |
| `docs/plugins.md` | Installed plugin/connector **names only** (no tokens). |
| `docs/ENVIRONMENT.md` | High-level box path map. |
| `routines/` | Export note (automations folder was empty). |

### How to restore skills

1. **Pointer skills (Drive-backed):** Open  
   https://drive.google.com/drive/folders/1f7nTYNd2jdMLVeh1PCV7ajmLXfTFxYah  
   Copy the full skill folders into your own Drive / local mirror, then re-import or sync with your bot’s skill library workflow (`ai-skills` / 共用 AI skills 雙向同步).
2. **Self-contained skills** (e.g. `grok-bridge`, bank-balance skill folder): Copy each `skills/<name>/SKILL.md` into your bot’s user-skills / workflows directory (on a Grok Bot box this is typically `/home/box/agent-data/workflows/<name>/SKILL.md`), or paste via the product’s skill-authoring / import UI.
3. **Reconnect plugins** listed in `docs/plugins.md` in the new environment (Gmail, Calendar, Drive, Sheets, Outlook, X, GitHub, etc.). You must sign in again; this backup has no credentials.
4. Recreate routines manually if needed — none were present on 總經理 at export time.

### Secrets policy

Excluded by design: passwords, cookies, `box-secrets` / `host-secrets`, vault data, full emails, addresses, phones, cards, `store.db`, `.env`. Company-specific names in routines/skills were generalized where found.

---

## 繁體中文

### 這是什麼

可攜帶的技能定義與連接器清單備份，方便在新環境還原 **skills** 與記得要接哪些 **plugins**。  
**不是**完整虛擬機映像，也**不含**登入密鑰、cookies、vault、郵件全文。

### 目錄說明

| 路徑 | 內容 |
|------|------|
| `skills/` | 自 `workflows/*/SKILL.md` 匯出的使用者技能（22 個）。多數為 Google Drive 指標檔。 |
| `skills/DRIVE_LIBRARY_NOTE.md` | 共用 Drive 資料夾連結說明。 |
| `agent-profile/` | 「總經理」的 `profile.json` / `settings.json`（已清理）。 |
| `docs/plugins.md` | 已安裝連接器**名稱**（無 token）。 |
| `docs/ENVIRONMENT.md` | box 路徑高階說明。 |
| `routines/` | 匯出當時 automations 為空的說明。 |

### 如何還原 skills

1. **Drive 指標型技能：** 開啟  
   https://drive.google.com/drive/folders/1f7nTYNd2jdMLVeh1PCV7ajmLXfTFxYah  
   將完整 skill 資料夾複製到自己的 Drive／本機鏡像，再用 bot 的技能庫同步流程（`ai-skills`）匯入。
2. **完整寫在本備份內的技能**（如 `grok-bridge`、網銀餘額查詢資料夾）：把 `skills/<name>/SKILL.md` 放到新環境的 workflows 目錄（Grok Bot box 通常為 `/home/box/agent-data/workflows/<name>/SKILL.md`），或透過產品的 skill 匯入介面貼上。
3. 依 `docs/plugins.md` **重新連接** Gmail、Calendar、Drive、Sheets、Outlook、X、GitHub 等；必須重新登入，本備份不含憑證。
4. Routines 需自行重建——匯出時「總經理」的 automations 資料夾是空的。

### 已排除的機密

刻意不包含：密碼、cookies、`box-secrets`／`host-secrets`、vault、郵件全文、住址、電話、信用卡、`store.db`、`.env`。例行流程／技能中的公司專名已盡量改成通用示例。

