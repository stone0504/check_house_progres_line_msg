# 🏠 吉美使用執照網頁監控系統

自動監控台中市政府吉美建案使用執照網頁，當有更新時立即透過 LINE 發送通知與截圖。

## 📋 功能特色

- ⏰ **定期監控**：自動定時檢查網頁是否有更新
- 🔔 **即時通知**：透過 LINE Messaging API 即時發送通知訊息
- 📸 **自動截圖**：使用 Selenium 自動截取網頁畫面
- 🖼️ **圖片上傳**：自動上傳截圖至 ImgBB 並附在 LINE 訊息中
- 🔍 **變更偵測**：使用 MD5 Hash 精確偵測網頁內容變更

## 🚀 快速開始

### 環境需求

- Python 3.7+
- Google Chrome 瀏覽器
- ChromeDriver（與 Chrome 版本相容）

### 安裝步驟

1. **克隆專案**
   ```bash
   git clone <repository-url>
   cd check_house_licence_line
   ```

2. **安裝相依套件**
   ```bash
   pip install -r requirements.txt
   ```

3. **配置設定檔**
   
   複製 `config_demo.json` 為 `config.json`：
   ```bash
   cp config_demo.json config.json
   ```

4. **填寫設定**
   
   編輯 `config.json` 並填入必要資訊：
   ```json
   {
       "LINE_ACCESS_TOKEN": "你的 LINE Bot Access Token",
       "USER_ID": "接收通知的 LINE 使用者或群組 ID",
       "LINE_API_URL": "https://api.line.me/v2/bot/message/push",
       "homepage_url": "https://tccmoapply.dba.tcg.gov.tw/tccmoapply/",
       "target_url": "https://tccmoapply.dba.tcg.gov.tw/tccmoapply/maliapp/asp/asp01_f000.jsp?YY=115&NO=8014&KIND=12&CG=00",
       "IMGBB_API_KEY": "你的 ImgBB API Key",
       "check_interval": 7200
   }
   ```

### 執行程式

```bash
python3 main.py
```

程式會開始監控，每 2 小時（7200 秒）檢查一次網頁。

## ⚙️ 設定說明

### 取得 LINE Bot Access Token

1. 前往 [LINE Developers Console](https://developers.line.biz/)
2. 建立 Messaging API Channel
3. 在「Messaging API」頁籤中取得 **Channel Access Token**
4. 將 LINE Official Account 加入你的群組
5. 取得群組的 `USER_ID`（可透過 [LINE Bot Designer](https://developers.line.biz/console/) 取得）

### 取得 ImgBB API Key

1. 前往 [ImgBB API](https://api.imgbb.com/)
2. 註冊帳號並登入
3. 點擊「Get API Key」
4. 填寫表單並取得 API Key

詳細設定步驟請參考：[ImgBB 設定指南](./docs/ImgBB設定指南.md)

## 📁 專案結構

```
check_house_licence_line/
├── main.py              # 主程式 - 監控邏輯與訊息發送
├── screenshot.py        # 截圖模組 - 使用 Selenium 截取網頁
├── config.json          # 配置檔（需自行建立，不納入版本控制）
├── config_demo.json     # 配置範本
├── README.md            # 專案說明
├── LICENCE              # MIT 授權
├── .gitignore           # Git 忽略檔案設定
└── requirements.txt     # Python 相依套件清單
```

## 🔧 主要模組說明

### `main.py`

核心監控程式，包含以下功能：

- **`get_page_content()`**：取得目標網頁內容
- **`get_content_hash()`**：計算內容的 MD5 Hash
- **`send_message()`**：發送 LINE 文字訊息
- **`send_image()`**：發送 LINE 圖片訊息
- **`upload_image_to_imgbb()`**：上傳截圖至 ImgBB

### `screenshot.py`

網頁截圖模組：

- 使用 Selenium WebDriver 控制 Chrome
- 先訪問首頁建立 Session
- 再前往目標頁面進行截圖
- 支援 Headless 模式（背景執行）

## 📦 相依套件

```
requests>=2.31.0
beautifulsoup4>=4.12.0
selenium>=4.15.0
```

建立 `requirements.txt`：
```bash
pip freeze > requirements.txt
```

## 🔍 運作流程

```
1. 程式啟動
   ↓
2. 讀取 config.json 配置
   ↓
3. 取得目標網頁內容
   ↓
4. 計算內容 Hash 值
   ↓
5. 與上次 Hash 比較
   ├─ 相同 → 無變更
   └─ 不同 → 有更新！
       ├─ 使用 Selenium 截圖
       ├─ 上傳圖片至 ImgBB
       ├─ 發送 LINE 文字通知
       └─ 發送 LINE 圖片訊息
   ↓
6. 等待設定的時間間隔
   ↓
7. 回到步驟 3（循環執行）
```

## 🎯 使用情境

本程式專為監控**台中市政府吉美建案使用執照申請進度**而設計。當使用執照網頁有任何更新時，會立即透過 LINE 通知，讓你不會錯過任何重要更新。

## ⚠️ 注意事項

- ⚡ **檢查頻率**：預設為 2 小時檢查一次，請勿設定過於頻繁以避免對目標網站造成負擔
- 🌐 **網路穩定性**：需保持網路連線穩定，建議在穩定的環境中執行
- 🔐 **敏感資訊**：`config.json` 包含 API Token，請勿將其上傳至公開倉庫
- 📜 **合法使用**：請遵守目標網站的使用條款與相關法規

## 🐛 常見問題

### Q: 執行時出現 `ChromeDriver` 錯誤
**A:** 請確認已安裝 Chrome 瀏覽器，並下載對應版本的 [ChromeDriver](https://chromedriver.chromium.org/)。

### Q: LINE 訊息發送失敗
**A:** 檢查 `LINE_ACCESS_TOKEN` 和 `USER_ID` 是否正確，以及 LINE Bot 是否已加入目標群組。

### Q: ImgBB 圖片上傳失敗
**A:** 確認 `IMGBB_API_KEY` 是否正確，以及網路連線是否正常。

### Q: 想要增加檢查頻率
**A:** 修改 `config.json` 中的 `check_interval` 值（單位：秒）。

## 📝 授權條款

本專案採用 [MIT License](./LICENCE) 授權。

## 🤝 貢獻

歡迎提交 Issue 或 Pull Request！

## 📧 聯絡方式

如有任何問題或建議，歡迎開 Issue 討論。

---

**⭐ 如果這個專案對你有幫助，歡迎給個星星支持！**