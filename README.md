# Seoul 2027 PWA Pro

## 新增功能
- 全站行程搜尋
- 景點收藏
- 每日花費記錄
- 行李清單
- 深色模式
- GPS 定位並開啟 Google Maps
- 手動韓元／澳元換算
- 離線快取與手機安裝

## 免費部署
最簡單：Netlify Drop
1. 解壓 ZIP。
2. 將 `seoul-2027-pwa-v2` 資料夾拖曳到 Netlify Drop。
3. 系統會自動提供 `https://xxxxx.netlify.app` 網址。

也可使用 GitHub Pages 或 Cloudflare Pages。

## 注意
- PWA 必須使用 HTTPS 或 localhost。
- Google Maps、Naver Map 和 GPS 地圖頁面需要網絡。
- 匯率欄位是手動輸入，避免依賴付費 API。
- 更新網站後，若手機仍顯示舊版，關閉 App 再重新開啟，或清除網站資料。
