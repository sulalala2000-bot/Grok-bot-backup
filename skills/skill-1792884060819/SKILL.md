---
name: 台銀與網銀餘額查詢
description: 當使用者要查臺灣銀行個人網銀、臺銀 e企合成網（公司戶）、國泰世華 CUBE 網銀餘額，或要一次彙整這幾家帳戶餘額時使用。
---
# 台銀與網銀餘額查詢

## 範圍
- 只查詢餘額／帳戶總覽，**不轉帳、不改設定、不核准交易**。
- 登入順序一律遵循 `sign-in`：Vault login → 密碼管理器 → Secure Form（`kind: login`）→ 必要時 `request_box_help`。
- 機密用 `BrowserSecureFill`；驗證碼／圖形碼由 computerUse 自行辨識；Email／簡訊 OTP 優先從已連線信箱讀取，否則用一欄 Secure Form（`submitAfterFill`）。

## 共通步驟
1. `box-desktop` → computerUse 開啟該行登入 URL；若已登入則直接進帳戶總覽並回報餘額。
2. 登入頁出現時：回報欄位 refs／建議 CSS selector；對使用者送 Secure Form（可存 Vault），填完後 computerUse 用 fill references 填入、過驗證、點登入。
3. iframe 內欄位：snapshot ref 常失敗，改用頁面實際 `id`／CSS selector 再 `BrowserSecureFill`。
4. 回報：帳號（可遮罩中段）、戶名／類型、餘額（可用／帳面若有）、幣別、合計；附總覽截圖。
5. 「信任裝置」預設選暫時不用，除非使用者要求記住。
6. 密碼到期／變更提醒：選「下次再變更」，除非使用者要求立刻改。

## 臺灣銀行個人網銀
- URL：`https://ebank.bot.com.tw/`
- 欄位：身分證字號、使用者代號、使用者密碼、圖形驗證碼；確認登入。
- 已知可用 selector（若 ref 失敗）：`#CustID`、`#UserId`、`#UserPswd`。
- 成功後在「我的帳戶」讀各戶餘額與合計。

## 臺灣銀行 e企合成網（公司戶／好e企）
- URL：`https://necomb.bot.com.tw/BOT_CIB_B2C_WEB/common/login/Login_1.faces`
- 欄位：統一編號、使用者代號、使用者密碼、圖形驗證碼；登入。
- 成功後走帳務查詢總覽 → 存款帳戶 → 歸戶總覽。

## 國泰世華 CUBE 網銀
- URL：`https://www.cathaybk.com.tw/MyBank/Home/Login`（一般登入；必要時 Classics）。
- 欄位：身分證字號、用戶代號、網銀密碼（此頁通常無圖形驗證碼）。
- 兩步驟驗證：裝置不在信任清單時可選簡訊或 Email；Email 驗證碼格式常為 `AEBC-` + 後 6 碼（頁面已顯示前綴，只填後 6 碼）。
- 成功後讀「臺幣帳戶總覽」；資產總覽上的淨資產／投資／信用卡數字可一併摘要，但標明與存款餘額不同。

## 一次查多家
依使用者指定順序逐家登入查詢；每家獨立 session／Vault 項目；最後給一份合計摘要（各行分開列，勿混加不同銀行）。
