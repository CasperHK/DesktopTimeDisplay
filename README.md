# Desktop Time Display

**DesktopTimeDisplay** 是一個專為 Linux (特別是 Raspberry Pi 5 / Wayland 環境) 設計的極簡、高效能懸浮時鐘。它採用 **Go** 語言編寫，並使用 **Gio UI** 進行底層圖形渲染，確保在佔用極低系統資源的情況下，實現「無框」、「置頂」且「零延遲」的顯示效果。

---

## 🛠 技術棧與哲學

* **語言:** Go (高效能、單一二進位檔)
* **渲染架構:** [Gio UI](https://gioui.org/) (Immediate Mode GUI，極致效能)
* **目標環境:** Linux (Wayland/X11), 針對 ARM64 架構最佳化
* **設計理念:** 拋棄傳統臃腫的 GUI 框架，直接與視窗管理系統對接，實現純粹的視覺顯示。

---

## 🚀 核心優勢

* **極致輕量:** 相較於 `tty-clock` 或基於瀏覽器的時鐘，記憶體佔用極低。
* **原生渲染:** 跳過終端機層級的權限限制，直接與視窗管理器互動。
* **無框設計:** 針對桌面懸浮需求設計，視覺上乾淨俐落。
* **跨架構編譯:** 在 x86 開發機上一鍵交叉編譯，無縫部署至 Pi 5。

---

## 🛠 快速開始

### 編譯與部署 (從 x86 到 Pi 5)

在您的開發機上執行以下指令進行交叉編譯：

```bash
# 編譯為 Pi 5 (ARM64) 可執行檔
CGO_ENABLED=1 GOOS=linux GOARCH=arm64 go build -o DesktopTimeDisplay

```

將執行檔複製到 Pi 5：

```bash
scp DesktopTimeDisplay pi@<your-pi-ip>:~/

```

### 執行

在 Pi 5 上運行：

```bash
chmod +x DesktopTimeDisplay
./DesktopTimeDisplay

```

---

## ⚙️ 進階：強制置頂 (Wayland / Wayfire)

若您使用 Pi 5 預設的 Wayfire 桌面，建議在 `~/.config/wayfire.ini` 中添加規則，以實現自動置頂：

```ini
[window-rules]
clock_always_on_top = on created if app_id is "DesktopTimeDisplay" then set above

```

---

## 📦 專案結構

```text
DesktopTimeDisplay/
├── main.go           # 核心邏輯 (Gio UI 渲染迴圈)
├── go.mod            # 依賴管理
└── README.md         # 說明文件

```

---

## 💡 開發者筆記

本專案採用 **Immediate Mode (IMGUI)** 渲染模式，意味著每一幀都由代碼定義。若您未來需要加入日期、系統負載資訊（如 CPU 溫度）或是自訂主題顏色，只需在 `loop` 函數中擴充繪製邏輯，無需處理複雜的 DOM 或 UI 元件狀態更新。
