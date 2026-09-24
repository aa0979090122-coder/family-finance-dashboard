# 我們家的錢

GitHub Pages 網站，登入 Google 後讀取私人「帳務管理」試算表。公開檔案不包含家人的財務紀錄，也不包含租金假設金額；授權憑證只保留在瀏覽器記憶體。

目前版本：可互動預覽與讀取程式已完成。Google OAuth 用戶端 ID 尚未設定，因此正式登入與同步尚未驗證，部署檢查會擋下未設定的版本。

## 一次性設定

1. 在 Google Cloud 選擇或建立專案，啟用 Google Sheets API，設定 Google Auth Platform 的同意畫面及家人測試帳號。
2. 建立「網頁應用程式」OAuth 用戶端。授權 JavaScript 來源填最終 GitHub Pages 的來源，例如 `https://你的帳號.github.io`，不要加儲存庫路徑。需要本機測試時另加 `http://127.0.0.1:4173`。
3. 將公開的 OAuth 用戶端 ID 提供給建置程序。不使用、不上傳 client secret。使用者授權範圍為 Google 試算表唯讀；這個 OAuth 範圍能讀該使用者有權存取的試算表，程式固定只請求指定家庭記帳表。
4. 在工作目錄執行 `FAMILY_GOOGLE_CLIENT_ID=... node work/family_web/prepare_pages.mjs`。程式只產生無財務資料的公開版本，不更動本機預覽資料。
5. 只將本資料夾推到指定儲存庫，Pages 選擇 GitHub Actions，設定 repository variable `PAGES_READY=true` 後手動啟動部署。不要上傳相鄰的本機預覽或原始資料目錄。
6. 打開 Pages，登入有權讀取試算表的帳號。驗證改一格後自動更新、無權限帳號被拒、登出後財務資料消失，再交給家人使用。

## 平常怎麼更新

- 照舊填 `2026支出` 等年度分頁的月度紀錄。網站讀 A1:AM75，忽略舊儀表板分頁。不要移動原表欄位；格式不符會停止更新並提示。
- 「網站設定」保留租金生效月份與每月合計，匯款已包含在總額內。金額改變時新增生效列，保留舊資料；日期格式為 YYYY-MM。設定讀 A1:C30。
- 登入且頁面開啟時，每 60 秒重讀；頁面在背景時暫停。授權過期後需再次登入，不保證無限期免登入。
- 本月標示還在記帳。未来月份預填值不當作歷史。年度費用除以 12 作為「預留」，不冒充當月實付。空白顯示未填。
- 朗讀使用裝置的中文語音，實際可用聲音取決於瀏覽器與系統。

## 來源與驗證

資料來源為使用者指定的 Google 試算表，原分頁未改寫；新增「網站設定」。現有預覽已核對金額、補估防重複、開關、月份切換與試算。正式 OAuth、60 秒同步及家人帳號授權需要設定完成後實測。

參考：[Google token model](https://developers.google.com/identity/oauth2/web/guides/use-token-model)、[GitHub Pages 部署](https://docs.github.com/en/pages/getting-started-with-github-pages/using-custom-workflows-with-github-pages)。
