# 依洛嘉國際 · 產品展示圖系統

> Product Showcase · Marketing System v3
> 上傳產品照 →（可選）去背 → 選風格/平台 → OpenAI 分析賣點並寫出「針對性出圖提示詞」→ 貼到 ChatGPT／Sora，或用 gpt-image-1 直接自動出圖。

線上網址：<https://elgtw99-web.github.io/picture/>
原始碼倉庫：`elgtw99-web/picture`（GitHub Pages，`main` 分支）

---

## 這套系統做什麼

一頁式（單一 `index.html`）的產品行銷圖工具。核心價值是**保留你真實的產品照片，只讓 AI 生成周圍的場景與氛圍**，避免 AI 把產品外觀、包裝、標籤畫錯。並依「產品賣點 × 目標客群 × 投放平台尺寸 × 風格主題」四個維度，量身寫出可直接出圖的提示詞。

**v3 重點更新**：整套改用**單一 OpenAI 金鑰**完成分析與出圖，移除了原本不穩定的 Anthropic 代理（先前「產生失敗 / Cannot read properties of undefined」的來源）。去背改為**選填**——gpt-image-1 可直接把原圖合成到新場景，不去背也行。

## 操作流程

1. **上傳產品主圖**。
2. **去背（選填）**：可用免費 AI 去背或 remove.bg；不去背也可以，gpt-image-1 會直接替換背景。
3. **上傳標籤／輸入成分說明**（供 AI 分析賣點與成分）。
4. **選擇風格主題**：大理石奢金、東方水墨、玫瑰輕奢、植萃自然、黑金極奢、薄霧仙境。
5. **細節選項**、**目標客群**（選填）。
6. **選投放平台／尺寸**：蝦皮 1:1、IG 4:5、限動 9:16、官網 16:9、通用方形。
7. **填 OpenAI API Key**（分析與出圖都用這一把）。

## 兩種出圖方式

**手動（用你現有的 ChatGPT／Sora）**

按「✦ 產生 AI 出圖提示詞」→ 系統回傳四段提示詞（白底主圖、成分賣點情境、質感深度、夢境情境），每段可一鍵複製。用「⬇ 下載產品照」拿到圖檔 → 到 ChatGPT／Sora 上傳該圖 → 貼上提示詞送出。

**自動（gpt-image-1，按張計費）**

每張提示詞卡片上的「✦ 自動出圖」會呼叫 `gpt-image-1` 的 `images/edits`，附上你的產品照生成場景，完成後可下載 PNG。

> 另保留「⚡ 快速預覽（Canvas 版四張圖）」按鈕，純前端合成，想快速看排版時可用，不需任何 API。

## 金鑰與費用

- OpenAI 金鑰只存在你本機瀏覽器的 `localStorage`，不會上傳到任何伺服器；請求由瀏覽器直接呼叫 `api.openai.com`。
- **分析**用 `gpt-4o-mini`（非常便宜）。**出圖**用 `gpt-image-1`（按張計費）。
- 注意：OpenAI 的 `gpt-image-1` 需要帳戶完成「Organization verification」才能使用；分析（gpt-4o-mini）則不需要。若自動出圖回報權限錯誤，請到 OpenAI 後台完成組織驗證。
- 取得金鑰：<https://platform.openai.com/api-keys>；remove.bg（選用去背）：<https://www.remove.bg/api>

## 技術架構

- 單檔 `index.html`，原生 HTML／CSS／JavaScript，無框架、無建置流程。
- `analyze()`：呼叫 OpenAI `chat/completions`（`gpt-4o-mini`，`response_format: json_object`，可讀標籤圖），回傳含 `sellingPoints`、`ingredients`、`painPoints`、`spec` 及 `imagePrompts`（四段針對性場景）的 JSON。
- `buildFinalPrompt()`：把場景包成含「保留產品本體＋平台比例＋風格基調」的完整提示詞。
- `generateWithOpenAI()`：`gpt-image-1` 的 `images/edits` 出圖；`apiSize` 已對應合法尺寸（1024x1024 / 1024x1536 / 1536x1024）。
- Canvas 繪圖函式（`drawImg1`–`drawImg4`）：快速預覽用的前端合成。

## 部署（更新線上網址）

本檔在你電腦的專案資料夾。要讓線上網址生效，需把 `index.html` 推到 GitHub `elgtw99-web/picture` 的 `main` 分支：

```bash
git clone https://github.com/elgtw99-web/picture.git
cp index.html picture/index.html
cd picture
git add index.html
git commit -m "v3：改用單一 OpenAI 金鑰（分析+出圖），去背選填，移除 Anthropic 代理"
git push origin main
```

或直接到 GitHub 網頁 → repo → `index.html` → Edit → 貼上內容 → Commit。GitHub Pages 約 1 分鐘後更新。

---

© 依洛嘉國際行銷使用 · 盜用必究
