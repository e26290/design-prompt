# Token-gen-prompt｜互動版模板

這必須建構為一個可用於生產環境的基礎，以便實現：

1. 一個非常龐大的 Figma 元件庫
2. 淺色/深色主題
3. 圓角模式切換
4. 未來使用 React 作為開發

Do not create component-specific tokens.
Do not simplify the system.
Do not skip scopes.
Do not leave typography partially connected.
Do not use raw values on text styles when variable binding is possible.

---

## PHASE 1 — 互動詢問

在建立任何內容之前，依序詢問以下問題。每題回答後繼續下一題，全部回答完畢再一次確認，然後進入 PHASE 2。

---

### 問題 1｜品牌主色（Primary Color）

詢問內容：

> 請輸入品牌主色。
>
> 可以直接給 hex（例：#057BC7），或描述顏色名稱（例：天藍色）。
> 如果給顏色名稱，我會自動產生一組適合 UI 使用的 ramp。

處理規則：
- 若給 hex，以該色為 /600 基準，向上向下推算 ramp（30、100、200、300、500、600、700、800）
- 若給顏色名稱，選一個適合 UI 的 hex 作為 /600，再推算 ramp
- ramp 命名：`color/primary/30` 至 `color/primary/800`

---

### 問題 2｜品牌副色（Secondary Color / Accent）

詢問內容：

> 請輸入品牌副色（Accent）。
>
> 可以給 hex 或顏色名稱，也可以回答「不需要」。
> 如果不需要，系統會略過副色相關的 token。

處理規則：
- 若有副色，推算 ramp（30、100、300、500、600、700）
- ramp 命名：`Color/Secondary/30` 至 `Color/Secondary/700`
- 若回答「不需要」，跳過所有 Secondary token，後續 Semantic 層也不建立副色別名，也不要使用明顯的強調色漸層。

---

### 問題 3｜中性色系（Neutral Family）

詢問內容：

> 請選擇中性色的色調方向：
>
> 1. **gray** — 純灰，無色溫（最通用）
> 2. **slate** — 略帶藍灰（適合科技、金融類產品）
> 3. **zinc** — 略帶暖灰（適合內容、媒體類產品）
> 4. **stone** — 略帶棕灰（適合品牌、生活風格類產品）
> 5. **品牌主色調色系灰** — 以灰色為主體，加入極少量品牌主色成分（飽和度控制在 5% 以內）
> 6. **指定色系** — 自行輸入一個 hex，我會以此推算完整 ramp
>
> 輸入數字或選項名稱皆可。

處理規則：
- 選 1–4：根據對應 family 的色調方向推算 Natural ramp
- 選 5：以灰色為主體，萃取品牌主色的色相，加入極少量（飽和度 5% 以內），視覺上仍以灰為主
- 選 6：請使用者輸入一個 hex，以該色作為 `Color/Natural/500` 基準，向上推算淡色（400、250、150、100、50），向下推算深色（700、800、900），`/0` 固定為 #FFFFFF，`/900` 固定為接近黑的深色
- 所有選項最終都產出相同的 ramp 階數（0、50、100、150、250、400、500、700、800、900）

---

### 問題 4｜字族（Font Family）

詢問內容：

> 請問要使用什麼字體？
>
> 預設為：
> - 中文：Noto Sans TC
> - 英文：Inter
>
> 可以直接回答「預設」，或告訴我要替換的字體名稱。
> 例如：「中文改用 Noto Serif TC、英文改用 Geist」
>
> ⚠ 若指定的字體在 Figma 中不可用，會自動退回預設值。

處理規則：
- 中文字族 → `Font/Family/TW`
- 英文字族 → `Font/Family/EN`
- 若使用者只指定一種，另一種維持預設
- 若使用者回答「預設」，兩者均使用預設值

---

### 問題 5｜裝置 Breakpoint

詢問內容：

> 請輸入裝置的畫面寬度。
>
> 預設：Desktop 1440px、Mobile 375px
>
> 是否需要加入 Tablet 斷點？
> - 回答「不需要」或「預設」→ 僅建立 Desktop / Mobile 兩個 Mode
> - 回答「需要」→ 額外建立 Tablet Mode，預設寬度 768px（可自訂）
>
> ⚠ 若建立 Tablet Mode，Font / Spacing / Radius 預設複製 Desktop 的值，如有需要可在建立後手動調整。

處理規則：
- Desktop 寬度 → `layout/width` Desktop Mode 值
- Mobile 寬度 → `layout/width` Mobile Mode 值
- 若有 Tablet → `layout/width` Tablet Mode 值，預設 768px
- `layout/desktop-open`、`layout/mobile-open`、`layout/tablet-open`（若有）記錄各裝置啟用狀態（boolean）

---

### 問題 6｜色彩模式設定

本問題分三步驟詢問，依序進行。

---

**問題 6-1｜主要 Mode**

詢問內容：

> 你的產品主要以哪個模式為主？
>
> - **Light Mode** — 白底為主，適合大多數 B2B、工具型、內容型產品
> - **Dark Mode** — 深底為主，適合影音、遊戲、創作工具類產品

處理規則：
- 記錄主要 Mode 作為後續配色的基準
- `Functional：Tokens` 的主 Mode 以此為準建立

---

**問題 6-2｜是否需要第二套 Mode**

詢問內容：

> 是否需要同時提供第二套配色 Mode？
>
> - 回答「不需要」→ `Functional：Tokens` 僅建立單一 Mode
> - 回答「需要」→ 繼續問題 6-3

處理規則：
- 若回答「不需要」，略過問題 6-3，直接進入確認環節
- 若回答「需要」，繼續問題 6-3

---

**問題 6-3｜第二套 Mode 的配色方向**

本題只在問題 6-2 回答「需要」時詢問。

詢問內容：

> 品牌主色（Primary ramp）在兩個 Mode 下保持相同色碼不變，改變的是底色——也就是整個介面的「舞台」。
>
> 根據你的品牌主色，以下是幾個推薦的底調方向，請選擇一個：
>
> [AI 根據問題 1 的品牌主色，動態產生 3–4 個推薦選項，每個選項同時包含情境描述與對比度數值]
>
> 例如品牌主色為檸檬綠（#A8D400）時，AI 應列出：
>
> 1. **深墨綠底** `#1A2E0A` — 與主色同色系，自然有機感強，適合農業、健康、環保類產品｜主色對比度 **7.1:1** ✅ WCAG AAA
> 2. **深炭灰底** `#1C1E1A` — 中性現代感，主色跳色明顯不搶戲，適合科技工具、SaaS 類產品｜主色對比度 **8.3:1** ✅ WCAG AAA
> 3. **深橄欖底** `#222B10` — 暖綠色溫，整體色調統一沉穩，適合精品、生活風格類產品｜主色對比度 **6.2:1** ✅ WCAG AA
> 4. **深黑底** `#141414` — 無色溫，主色對比最強烈，適合科技感或極簡風格產品｜主色對比度 **9.1:1** ✅ WCAG AAA
>
> 輸入數字即可。

處理規則：
- AI 必須根據問題 1 的品牌主色色相、明度、彩度，判斷哪些底調在視覺上協調且對比足夠，再列出 3–4 個選項
- 每個選項必須同時提供：底色 hex、使用情境描述、主色在該底色上的對比度數值與 WCAG 等級
- 對比度未達 WCAG AA（4.5:1）的底調不列入選項
- 不推薦與主色色相過於相近且明度接近的底調（容易吃色）
- 設計師選定後，AI 根據該底調推算 `Color/Dark/*` ramp 並加入 `Base：Style`，`Functional：Tokens` 的第二套 Mode 再 alias 這裡，不寫死色碼
- 第二套配色不是單純反轉主 Mode 的 ramp，而是重新根據底調配置每個 token，確保視覺效果正確

---

### 確認環節

所有問題回答完畢後，整理並輸出以下摘要請使用者確認：

```
以下是你的設定，確認後將開始建立 token 系統：

品牌主色：[hex 值]
品牌副色：[hex 值 或「無」]
中性色系：[選項名稱]
中文字族：[字體名稱]
英文字族：[字體名稱]
Desktop 寬度：[px]
Tablet 寬度：[px 或「無」]
Mobile 寬度：[px]
主要 Mode：[Light / Dark]
第二套 Mode：[底調方向名稱 或「無」]

請確認以上內容，或告訴我需要修改的地方。
```

確認後進入 PHASE 2。

---

## PHASE 2 — 建立 Primitive（Base）Collection

**Collection 名稱：** `Base：Style`
**Mode 數量：** 1 個 Mode
**說明：** 所有原始值。其他 Collection 的 token 皆 alias 這裡，不重複寫值。

---

### 2-A｜Color — Primary

根據問題 1 的輸入推算完整 ramp。

Token 命名格式：

```
Color/Primary/30
Color/Primary/100
Color/Primary/200
Color/Primary/300
Color/Primary/500
Color/Primary/600   ← 品牌主色基準
Color/Primary/700
Color/Primary/800
```

- Type：color
- Scope：ALL_FILLS

---

### 2-B｜Color — Secondary

若問題 2 有副色，推算以下 ramp：

```
Color/Secondary/30
Color/Secondary/100
Color/Secondary/300
Color/Secondary/500   ← 副色基準
Color/Secondary/600
Color/Secondary/700
```

- Type：color
- Scope：ALL_FILLS

若問題 2 回答「不需要」，略過此區塊。

---

### 2-C｜Color — Natural

根據問題 3 的色系方向推算：

```
Color/Natural/0    → #FFFFFF
Color/Natural/50
Color/Natural/100
Color/Natural/150
Color/Natural/250
Color/Natural/400
Color/Natural/500
Color/Natural/700
Color/Natural/800
Color/Natural/900
```

- 選項 5（品牌主色調色系灰）：以灰色為主體，品牌主色飽和度控制在 5% 以內，視覺上仍以灰為主
- Type：color
- Scope：ALL_FILLS

---

### 2-D｜Color — Status

固定建立以下狀態色，不詢問使用者：

```
Color/Global/Success/100
Color/Global/Success/600
Color/Global/Success/700

Color/Global/Danger/100
Color/Global/Danger/600
Color/Global/Danger/700

Color/Global/Warning/100
Color/Global/Warning/500
Color/Global/Warning/600

Color/Global/Alpha/Black-50   → #000000 @ alpha 50%
Color/Global/Alpha/White-50   → #FFFFFF @ alpha 50%
```

- Type：color
- Scope：ALL_FILLS

---

### 2-E｜Color — Dark Ramp（第二套底色）

若問題 6-2 回答「需要」，根據問題 6-3 設計師選定的底調方向，推算完整的深底色 ramp：

```
Color/Dark/50    ← 最淡，用於 raised 層（卡片、浮層）
Color/Dark/100   ← 次淡，用於 ghost hover
Color/Dark/200   ← 底色基準（設計師選定的底調 hex）
Color/Dark/300   ← 略深，用於 canvas（最底層背景）
Color/Dark/400   ← 最深，用於強調邊框或深色 overlay
```

- 以設計師選定的底調 hex 作為 `Color/Dark/200` 基準，向上推算更亮的層次（50、100），向下推算更深的層次（300、400）
- 各階之間明度差距約 5–8%，確保層次清晰可辨
- Type：color
- Scope：ALL_FILLS

若問題 6-2 回答「不需要」，略過此區塊。

---

### 2-F｜Font — Family

```
Font/Family/TW   → 問題 4 中文字族
Font/Family/EN   → 問題 4 英文字族
```

- Type：string
- Scope：FONT_FAMILY

---

### 2-G｜Font — Weight

```
Font/Weight/Regular   → "Regular"
Font/Weight/Medium    → "Medium"
Font/Weight/Bold      → "Bold"
```

- Type：string
- Scope：FONT_STYLE

---

### 2-G｜Font — Size

```
Font/Size/xs       → 12
Font/Size/sm       → 14
Font/Size/md       → 16
Font/Size/lg       → 18
Font/Size/xl       → 20
Font/Size/2xl      → 24
Font/Size/3xl      → 28
Font/Size/4xl      → 32
Font/Size/5xl      → 48
Font/Size/Display  → 64
```

- Type：number
- Scope：FONT_SIZE

---

### 2-I｜Radius

```
Radius/sm    → 12
Radius/md    → 24
Radius/xl    → 48
Radius/pill  → 1000
```

- Type：number
- Scope：CORNER_RADIUS

---

### 2-J｜Spacing

```
Spacing/1   → 4
Spacing/2   → 8
Spacing/3   → 12
Spacing/4   → 16
Spacing/5   → 20
Spacing/6   → 24
Spacing/8   → 32
Spacing/10  → 40
Spacing/12  → 48
Spacing/16  → 64
Spacing/18  → 72
Spacing/20  → 80
Spacing/30  → 120
```

- Type：number
- Scope：GAP、PADDING

---

## PHASE 3 — 建立 Semantic（Functional）Collection

**Collection 名稱：** `Functional：Tokens`
**Mode 數量：**
- 問題 6-2 選「不需要」→ 1 個 Mode（主要 Mode）
- 問題 6-2 選「需要」→ 2 個 Mode（主要 Mode + 第二套 Mode）

**說明：** 採用屬性導向語義結構，分為 Surface / Content / Border / Action / Status 五個群組。所有值 alias `Base：Style`，不寫死色碼。

若只有單一 Mode，以下所有表格僅建立主要 Mode 欄位的值。
若有第二套 Mode，Surface 背景層的深底色 alias `Color/Dark/*`（在 2-E 建立的 ramp），其餘 Content / Border / Action / Status 的 token 則根據底調重新選擇對應的 Primitive alias，確保視覺對比與可讀性，不直接反轉主 Mode 的 ramp。

以下表格以 Light 為主要 Mode、Dark 為第二套 Mode 示範，若主要 Mode 為 Dark 則對調。

---

### 3-A｜Surface 背景層

用於所有背景、容器、遮罩。

| Token 名稱 | Light alias | Dark alias |
|---|---|---|
| Surface/bg/canvas | Color/Natural/50 | Color/Natural/900 |
| Surface/bg/default | Color/Natural/0 | Color/Natural/800 |
| Surface/bg/raised | Color/Natural/0 | Color/Natural/700 |
| Surface/bg/overlay | Color/Global/Alpha/Black-50 | Color/Global/Alpha/Black-50 |
| Surface/bg/brand | Color/Primary/600 | Color/Primary/700 |
| Surface/bg/brand-subtle | Color/Primary/30 | Color/Primary/800 |

- Type：color
- Scope：FRAME_FILL、SHAPE_FILL

---

### 3-B｜Content 內容層

用於所有文字與圖示。

| Token 名稱 | Light alias | Dark alias |
|---|---|---|
| Content/text/primary | Color/Natural/900 | Color/Natural/0 |
| Content/text/secondary | Color/Natural/500 | Color/Natural/400 |
| Content/text/tertiary | Color/Natural/400 | Color/Natural/500 |
| Content/text/disabled | Color/Natural/250 | Color/Natural/700 |
| Content/text/on-brand | Color/Natural/0 | Color/Natural/0 |
| Content/text/brand | Color/Primary/600 | Color/Primary/300 |
| Content/text/link | Color/Primary/600 | Color/Primary/300 |
| Content/text/link-hover | Color/Primary/700 | Color/Primary/200 |
| Content/icon/default | Color/Natural/700 | Color/Natural/400 |
| Content/icon/brand | Color/Primary/600 | Color/Primary/300 |
| Content/icon/subtle | Color/Natural/400 | Color/Natural/500 |

- Type：color
- Scope：text → TEXT_FILL；icon → SHAPE_FILL

---

### 3-C｜Border 邊框層

用於所有線條與分隔線。

| Token 名稱 | Light alias | Dark alias |
|---|---|---|
| Border/default | Color/Natural/150 | Color/Natural/700 |
| Border/subtle | Color/Natural/100 | Color/Natural/800 |
| Border/strong | Color/Natural/250 | Color/Natural/500 |
| Border/brand | Color/Primary/600 | Color/Primary/300 |
| Border/focus | Color/Primary/300 | Color/Primary/500 |
| Border/error | Color/Global/Danger/600 | Color/Global/Danger/600 |

- Type：color
- Scope：STROKE_COLOR

---

### 3-D｜Action 操作層

用於按鈕、輸入框等互動元件的背景與狀態色。

| Token 名稱 | Light alias | Dark alias |
|---|---|---|
| Action/primary/default | Color/Primary/600 | Color/Primary/500 |
| Action/primary/hover | Color/Primary/700 | Color/Primary/400 |
| Action/primary/subtle | Color/Primary/30 | Color/Primary/800 |
| Action/primary/subtle-hover | Color/Primary/100 | Color/Primary/700 |
| Action/secondary/default | Color/Secondary/500 | Color/Secondary/400 |
| Action/secondary/hover | Color/Secondary/600 | Color/Secondary/300 |
| Action/ghost/default | Color/Natural/0 | Color/Natural/800 |
| Action/ghost/hover | Color/Natural/100 | Color/Natural/700 |
| Action/disabled/bg | Color/Natural/250 | Color/Natural/700 |
| Action/disabled/content | Color/Natural/400 | Color/Natural/500 |

若問題 2 選「不需要」，略過 `Action/secondary/*` 的建立。

- Type：color
- Scope：FRAME_FILL、SHAPE_FILL

---

### 3-E｜Status 狀態層

用於所有成功、警告、危險狀態。

| Token 名稱 | Light alias | Dark alias |
|---|---|---|
| Status/success/bg | Color/Global/Success/100 | Color/Global/Success/700 |
| Status/success/content | Color/Global/Success/700 | Color/Global/Success/100 |
| Status/success/emphasis | Color/Global/Success/600 | Color/Global/Success/600 |
| Status/warning/bg | Color/Global/Warning/100 | Color/Global/Warning/600 |
| Status/warning/content | Color/Global/Warning/600 | Color/Global/Warning/100 |
| Status/warning/emphasis | Color/Global/Warning/500 | Color/Global/Warning/500 |
| Status/danger/bg | Color/Global/Danger/100 | Color/Global/Danger/700 |
| Status/danger/content | Color/Global/Danger/600 | Color/Global/Danger/100 |
| Status/danger/emphasis | Color/Global/Danger/600 | Color/Global/Danger/600 |

- Type：color
- Scope：bg → FRAME_FILL / SHAPE_FILL；content → TEXT_FILL / SHAPE_FILL；emphasis → SHAPE_FILL

---

## PHASE 4 — 建立 RWD（Components）Collection

**Collection 名稱：** `RWD：Components`
**Mode 數量：**
- 問題 5 選「不需要 Tablet」→ 2 個 Mode（Desktop、Mobile）
- 問題 5 選「需要 Tablet」→ 3 個 Mode（Desktop、Tablet、Mobile）

**說明：** Font、Spacing、Radius 全部跟著 Device Mode 切換。所有值 alias `Base：Style`。

---

### 4-A｜Layout 裝置旗標

2 個 Mode 時：

```
layout/desktop-open   Desktop: 1  /  Mobile: 0
layout/mobile-open    Desktop: 0  /  Mobile: 1
layout/width          Desktop: [問題5 Desktop值]  /  Mobile: [問題5 Mobile值]
```

3 個 Mode 時（含 Tablet）：

```
layout/desktop-open   Desktop: 1  /  Tablet: 0  /  Mobile: 0
layout/tablet-open    Desktop: 0  /  Tablet: 1  /  Mobile: 0
layout/mobile-open    Desktop: 0  /  Tablet: 0  /  Mobile: 1
layout/width          Desktop: [值]  /  Tablet: [值]  /  Mobile: [值]
```

- Type：number（boolean 以 0/1 表示）
- Scope：layout/width → WIDTH_HEIGHT；其餘 → ALL_SCOPES

---

### 4-B｜Font 響應式字體

Desktop 往下縮一階到 Mobile。若有 Tablet，預設複製 Desktop 值。

| Token 名稱 | Desktop | Tablet（若有） | Mobile |
|---|---|---|---|
| Font/--display | alias Font/Size/Display（64） | alias Font/Size/Display（64） | alias Font/Size/5xl（48） |
| Font/--5xl | alias Font/Size/5xl（48） | alias Font/Size/5xl（48） | alias Font/Size/4xl（32） |
| Font/--4xl | alias Font/Size/4xl（32） | alias Font/Size/4xl（32） | alias Font/Size/3xl（28） |
| Font/--3xl | alias Font/Size/3xl（28） | alias Font/Size/3xl（28） | alias Font/Size/2xl（24） |
| Font/--2xl | alias Font/Size/2xl（24） | alias Font/Size/2xl（24） | alias Font/Size/xl（20） |
| Font/--xl | alias Font/Size/xl（20） | alias Font/Size/xl（20） | alias Font/Size/lg（18） |
| Font/--lg | alias Font/Size/lg（18） | alias Font/Size/lg（18） | alias Font/Size/md（16） |
| Font/--md | alias Font/Size/md（16） | alias Font/Size/md（16） | alias Font/Size/sm（14） |
| Font/--sm | alias Font/Size/sm（14） | alias Font/Size/sm（14） | alias Font/Size/sm（14） |
| Font/--xs | alias Font/Size/xs（12） | alias Font/Size/xs（12） | alias Font/Size/xs（12） |

- Type：number
- Scope：FONT_SIZE

---

### 4-C｜Spacing 響應式間距

若有 Tablet，預設複製 Desktop 值。

| Token 名稱 | Desktop | Tablet（若有） | Mobile |
|---|---|---|---|
| Spac/Btn/--px-lg | alias Spacing/5（20） | alias Spacing/5（20） | alias Spacing/3（12） |
| Spac/Btn/--px-md | alias Spacing/4（16） | alias Spacing/4（16） | alias Spacing/3（12） |
| Spac/Btn/--py | alias Spacing/3（12） | alias Spacing/3（12） | alias Spacing/2（8） |
| Spac/Section/--px | alias Spacing/20（80） | alias Spacing/20（80） | alias Spacing/12（48） |

- Type：number
- Scope：GAP、PADDING

---

### 4-D｜Radius 響應式圓角

若有 Tablet，預設複製 Desktop 值。

| Token 名稱 | Desktop | Tablet（若有） | Mobile |
|---|---|---|---|
| Radius/--card | alias Radius/xl（48） | alias Radius/xl（48） | alias Radius/md（24） |
| Radius/--btn | alias Radius/md（24） | alias Radius/md（24） | alias Radius/sm（12） |
| Radius/--tag | alias Radius/sm（12） | alias Radius/sm（12） | alias Radius/sm（12） |
| Radius/--pill | alias Radius/pill（1000） | alias Radius/pill（1000） | alias Radius/pill（1000） |

- Type：number
- Scope：CORNER_RADIUS

---

## PHASE 5 — 建立 Text Style

建立中文與英文兩套 Text Style，以 `TW/` 和 `EN/` 做區隔。每個屬性綁定對應的 RWD token（不手動輸入值）。

Line Height 規則：
- 單行文字（display、heading、title）使用 120%
- 多行文字（body、caption）使用 150%
- 若 Figma variable 不支援 % 單位，line-height 欄位留空，在 Style 建立後手動補上對應 % 值

Letter Spacing：所有 Style 固定設定 2%

### TW 中文字型組（綁定 Font/Family/TW）

| Style 名稱 | font-size alias | 字重 | Line Height |
|---|---|---|---|
| TW/display | Font/--display | Bold | 120% |
| TW/heading/h1 | Font/--5xl | Bold | 120% |
| TW/heading/h2 | Font/--4xl | Bold | 120% |
| TW/heading/h3 | Font/--3xl | Medium | 120% |
| TW/heading/h4 | Font/--2xl | Medium | 120% |
| TW/title/lg | Font/--xl | Medium | 120% |
| TW/title/md | Font/--lg | Medium | 120% |
| TW/body/lg | Font/--lg | Regular | 150% |
| TW/body/md | Font/--md | Regular | 150% |
| TW/body/sm | Font/--sm | Regular | 150% |
| TW/caption | Font/--xs | Regular | 150% |

### EN 英文字型組（綁定 Font/Family/EN）

| Style 名稱 | font-size alias | 字重 | Line Height |
|---|---|---|---|
| EN/display | Font/--display | Bold | 120% |
| EN/heading/h1 | Font/--5xl | Bold | 120% |
| EN/heading/h2 | Font/--4xl | Bold | 120% |
| EN/heading/h3 | Font/--3xl | Medium | 120% |
| EN/heading/h4 | Font/--2xl | Medium | 120% |
| EN/title/lg | Font/--xl | Medium | 120% |
| EN/title/md | Font/--lg | Medium | 120% |
| EN/body/lg | Font/--lg | Regular | 150% |
| EN/body/md | Font/--md | Regular | 150% |
| EN/body/sm | Font/--sm | Regular | 150% |
| EN/caption | Font/--xs | Regular | 150% |

---

## PHASE 6 — 品質檢核

完成後逐一確認以下項目，有任何不符合請修正後再回報：

- [ ] `Base：Style` 建立完成，包含 Color / Font / Radius / Spacing 四類 token
- [ ] `Functional：Tokens` 建立完成，採屬性導向結構（Surface / Content / Border / Action / Status）
- [ ] `Functional：Tokens` 所有值皆為 alias，無寫死色碼
- [ ] 若有第二套 Mode，配色根據選定底調方向重新配置，非直接反轉主 Mode ramp
- [ ] 若有第二套 Mode，所有 token 在該底調下對比度與可讀性符合視覺要求
- [ ] `RWD：Components` 建立完成，Mode 數量與問題 5 設定一致
- [ ] Font token 各 Mode 縮放正確
- [ ] Spacing token 各 Mode 縮小正確
- [ ] Radius token 各 Mode 縮小正確
- [ ] 若有 Tablet Mode，Font / Spacing / Radius 預設值與 Desktop 相同
- [ ] 所有 Variable Scope 已設定
- [ ] TW Text Style 共 11 個，綁定 Font/Family/TW
- [ ] EN Text Style 共 11 個，綁定 Font/Family/EN
- [ ] 所有 Text Style Letter Spacing 設定為 2%
- [ ] 單行 Text Style Line Height 120%、多行 Text Style Line Height 150%

完成後輸出以下摘要：

```
Token 系統建立完成。

品牌主色：[hex]
品牌副色：[hex 或「無」]
中性色系：[選項名稱]
中文字族：[字體名稱]
英文字族：[字體名稱]
Desktop 寬度：[px]
Tablet 寬度：[px 或「無」]
Mobile 寬度：[px]
主要 Mode：[Light / Dark]
第二套 Mode：[底調方向名稱 或「無」]

Collection 清單：
- Base：Style（1 個 Mode）
- Functional：Tokens（[1 或 2] 個 Mode）
- RWD：Components（[2 或 3] 個 Mode）

Text Style 清單：
- TW：display / h1 / h2 / h3 / h4 / title-lg / title-md / body-lg / body-md / body-sm / caption
- EN：display / h1 / h2 / h3 / h4 / title-lg / title-md / body-lg / body-md / body-sm / caption
```
