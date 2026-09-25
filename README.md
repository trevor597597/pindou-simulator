# 拼豆模拟器 · 拼豆图纸生成器

把相片轉成拼豆（perler / fuse beads）圖紙的**單檔網頁工具**：無任何依賴、無需後端、離線可用，
所有數據只存在瀏覽器 localStorage，唔會上傳任何嘢。

## 功能

- **上傳相片**：相冊／拖曳／貼上皆可，自動裁成正方形預覽
- **網格尺寸**：8–104 格（預設 52），底板規格 26×26 自動計算分板數
- **色板**：291 色 HSL 基礎色板，可切 72 / 96 / 144 / 221 / 238 / 291 色
- **量化**：中位切分（median cut）取代表色 → 吸附到色板；最大顏色數可調（預設 32）
- **抖動（Dithering）**：`关闭 · 纯色块` / `标准 · 抖动`（Floyd–Steinberg）/ `细腻 · 抖动`（Jarvis–Judice–Ninke）
  —— 誤差擴散把量化誤差攤去鄰格，用疏密混色模擬中間色，漸變唔再有色帶，遠看更似相片
- **畫布編輯**：畫筆／橡皮／吸管、撤銷重做（60 步）、縮放（15%–600%）與平移
- **統計**：拼豆總數、顏色數量、底板數量、逐色用量與百分比（可點選高亮）
- **匯出**：PNG 高清（40px/格）、SVG 矢量（帶色號文字與底板分隔線）、CSV 清單（含 BOM，Excel 中文正常）、打印圖紙
- **自動存檔**：圖紙、色板設定、抖動模式、來源縮圖全部存 localStorage，重開仍在

## 使用

直接用瀏覽器打開 `index.html`，或經 GitHub Pages 訪問。

## 檔案

| 檔案 | 說明 |
| --- | --- |
| `index.html` | 整個工具（HTML + CSS + JS 單檔，簡體 UI） |

## 開發備註

- 單檔設計，無建置步驟；`node --check` 可驗證 script 區塊語法
- 抖動核（`DITHER_KERNELS`）：Floyd–Steinberg（4 格，權重和 = 1）與 Jarvis–Judice–Ninke（12 格，權重和 = 1）；
  誤差必須守恆，否則會指數式爆走（曾用「誤差 ×1.55」實測逐像素 MSE 由 ~2000 爆到 ~19000）
- 偵錯介面：`window.PINDOU`（`S`、`generatePattern`、`medianCut`、`ditherCells`、`renderBoardCanvas`、`buildSVG`、`buildCSV` …）
- localStorage key：`pindou_pattern_v1`

## 手機注意

- iPhone 的 **HEIC** 相片 Chrome 解不到（Safari 可以），先在相冊「匯出成 JPG」再上傳
- 部分 Android 相冊會回空 MIME / `application/octet-stream`，本工具會照收再交由解碼器把關
