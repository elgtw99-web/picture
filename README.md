# 依洛嘉國際 · 產品展示圖系統

> Product Showcase · Marketing System v3
> 上傳產品照 → 去背 → 選風格/平台 → AI 產出「針對性出圖提示詞」→ 貼到 ChatGPT／Sora 或直接用 gpt-image-1 自動出圖。

線上網址：<https://elgtw99-web.github.io/picture/>
原始碼倉庫：`elgtw99-web/picture`（GitHub Pages，`main` 分支）

---

## 這套系統做什麼

一頁式（單一 `index.html`）的產品行銷圖工具。核心價值是**保留你真實的產品照片，只讓 AI 生成周圍的場景與氛圍**，避免 AI 把產品外觀、包裝、標籤畫錯。並依「產品賣點 × 目標客群 × 投放平台尺寸 × 風格主題」四個維度，量身寫出可直接出圖的提示詞。

## 操作流程

1. **上傳產品主圖**，可用內建去背（免費 AI 去背，或填 remove.bg 金鑰）。
2. **上傳標籤／輸入成分說明**（供 AI 分析賣點與成分）。
3. **選擇風格主題**：大理石奢金、東方水墨、玫瑰輕奢、植萃自然、黑金極奢、薄霧仙境。
4. **細節選項**：成分圖示、放射連線、植物裝飾、粒子、標語、規格欄、深色第三張。
5. **填目標客群**（選填，例：30–45 歲熟齡女性、抗老保濕）。
6. **選投放平台／尺寸**：蝦皮 1:1、IG 4:5、限動 9:16、官網 16:9、通用方形。
7. **填 Anthropic API Key**（分析與寫提示詞用）。
8. **選填 OpenAI API Key**（要自動出圖才需要）。

## 兩種出圖方式

**手動（免費，用你現有的 ChatGPT／Sora）**

按「✦ 產生 AI 出圖提示詞」→ 系統回傳四段提示詞（對應：白底主圖、成分賣點情境、質感深度、夢境情境）→ 每段可一鍵複製。用「⬇ 下載去背產品照」拿到圖檔 → 到 ChatGPT／Sora 先上傳該圖 → 貼上提示詞送出。

**自動（gpt-image-1，按張計費）**

在「08 · OpenAI API Key」填入金鑰後，每張提示詞卡片會出現「✦ 自動出圖」，直接呼叫 `gpt-image-1` 的 `images/edits`，附上你的去背產品照生成場景，完成後可下載 PNG。

> 另保留「⚡ 快速預覽（Canvas 版四張圖）」按鈕，純前端合成，想快速看排版時可用，不需出圖 API。

## 金鑰與隱私

- 所有金鑰只存在你本機瀏覽器的 `localStorage`，不會上傳到任何伺服器。
- Anthropic 分析請求經由 Cloudflare Worker 代理（`picture.elg-tw99.workers.dev`）轉發，避免瀏覽器 CORS 問題。
- OpenAI 出圖請求由瀏覽器直接呼叫 `api.openai.com`。

取得金鑰：
- Anthropic：<https://console.anthropic.com/keys>
- OpenAI：<https://platform.openai.com/api-keys>
- remove.bg（選用）：<https://www.remove.bg/api>

## 技術架構

- 單檔 `index.html`，原生 HTML／CSS／JavaScript，無框架、無建置流程。
- `analyze()`：呼叫 Claude（`claude-sonnet-4`）回傳 JSON，含 `sellingPoints`、`ingredients`、`painPoints`、`spec` 及新增的 `imagePrompts`（四段針對性場景）。
- `buildFinalPrompt()`：把場景包成含「保留產品本體＋平台比例＋風格基調」的完整提示詞。
- `generateWithOpenAI()`：`gpt-image-1` 出圖串接。
- Canvas 繪圖函式（`drawImg1`–`drawImg4`）：快速預覽用的前端合成。

## 部署（更新線上網址）

本檔在你電腦的專案資料夾。要讓線上網址生效，需把 `index.html` 推到 GitHub `elgtw99-web/picture` 的 `main` 分支：

```bash
git clone https://github.com/elgtw99-web/picture.git
cp index.html picture/index.html
cd picture
git add index.html
git commit -m "升級：新增針對性 AI 出圖提示詞與 gpt-image-1 串接"
git push origin main
```

或直接到 GitHub 網頁 → repo → `index.html` → Edit → 貼上內容 → Commit。GitHub Pages 約 1 分鐘後更新。

---

© 依洛嘉國際行銷使用 · 盜用必究
