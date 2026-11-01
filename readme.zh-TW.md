# Links

[English](./readme.md) | 繁體中文

一個超輕量、可自行託管的連結 landing page。

- 語義化 HTML
- SCSS 與現代原生 CSS
- 不使用 JavaScript 或前端框架
- Catppuccin Latte 與 Mocha
- Solid、mica 與 acrylic 三種材質

## Demo

專案頁面同時是即時元件展示與簡明文件：

[查看線上展示](https://undecv.github.io/Links/)

展開頁面下方的 **Preset recipes**，可以找到現成的社群與聯絡連結。

## 結構

每個 Links 頁面都使用相同的三層階層：

```text
lk-container
└── lk-section
    └── lk-block-*
```

- `lk-container` 是單欄頁面的框架。
- `lk-section` 將相關內容分組。
- `lk-block-*` 是實際的 UI 元件。

保持階層扁平。section 放在 container 裡，block 放在 section 裡；避免在 section 裡再放 section，或在獨立 block 裡再放另一個獨立 block。

依照原生元素的語義使用它們。section 可以是 `section`、`nav`、`address` 或 `details`；link block 即使看起來像按鈕，仍然是 `a`。

## 元件

| Class | 用途 |
| --- | --- |
| `lk-block-header` | 個人、專案或頁面標題 |
| `lk-block-link` | 主要導覽連結 |
| `lk-block-text` | 標題、段落、清單與行內連結 |
| `lk-block-figure` | 靜態或可點擊的圖片內容 |
| `lk-block-footer` | 來源、版權與頁尾資訊 |
| `lk-block-meta` | 精簡的次要導覽 |

`lk-block-icon`、`lk-block-content` 與 `lk-details-content` 是內部輔助 class，不是額外的階層。在 `details` section 裡，將 `summary` 之後的所有內容包進 `lk-details-content`，即可啟用原生 CSS 展開動畫。

### Icon

Header 或 link 的 icon slot 可以放一張圖片，或一個 Font Awesome icon：

```html
<span class="lk-block-icon" aria-hidden="true">
  <i class="fa-brands fa-github"></i>
</span>
```

```html
<span class="lk-block-icon">
  <img src="./avatar.jpg" width="48" height="48" alt="" />
</span>
```

加入 `is-rounded` 可以將圖片裁切成圓形。不需要 icon 時，直接省略整個 icon slot。

### Figure

如果整張圖片都應該可以點擊，將 `figure` 包在 anchor 裡：

```html
<a class="lk-block-figure" href="https://example.com">
  <figure>
    <img src="./image.jpg" alt="有意義的圖片說明" />
    <figcaption>可選的可見圖片註解。</figcaption>
  </figure>
</a>
```

靜態 figure 可以省略 anchor。圖片已有適當的 `alt` 說明時，`figcaption` 也不是必要的。

## 材質

在 `body` 選擇材質：

```html
<body data-material="acrylic" data-wallpaper-effect="material">
```

| 值 | 特性 |
| --- | --- |
| `solid` | 完全不透明的語義表面，不使用 backdrop blur |
| `mica` | 適度透明的表面與節制的模糊效果 |
| `acrylic` | 更透明的表面與更強的模糊效果 |

缺少 `data-material`，或使用無效值時，會退回 solid。

Catppuccin Latte 與 Mocha 會跟隨 `prefers-color-scheme`，不提供手動主題狀態。

## 桌布

從 HTML 宣告桌布，讓相對路徑繼續以文件位置為基準，部署在 subpath 下也能正常運作：

```html
<picture class="lk-wallpaper-source">
  <source
    media="(prefers-color-scheme: dark)"
    type="image/avif"
    srcset="
      ./assets/images/wallpaper-dark.1280.avif 1280w,
      ./assets/images/wallpaper-dark.1920.avif 1920w
    "
    sizes="100vw"
  />
  <source
    media="(prefers-color-scheme: dark)"
    srcset="./assets/images/wallpaper-dark.jpg"
  />
  <source
    type="image/avif"
    srcset="
      ./assets/images/wallpaper-light.1280.avif 1280w,
      ./assets/images/wallpaper-light.1920.avif 1920w
    "
    sizes="100vw"
  />
  <img
    class="lk-wallpaper"
    src="./assets/images/wallpaper-light.jpg"
    width="1920"
    height="1280"
    alt=""
    aria-hidden="true"
    fetchpriority="high"
  />
</picture>
```

照片桌布建議使用響應式 AVIF 或 WebP，並採用 `image.<pixel-width>.<format>` 命名格式。保留原始 JPEG 作為 fallback；照片使用 PNG 通常會更大。

`data-wallpaper-effect="material"` 會套用目前材質的 tint、blur 與 saturation。未設定時顯示原始桌布。

使用原生 `hidden` attribute 可以暫時停用桌布：

```html
<picture class="lk-wallpaper-source" hidden>...</picture>
```

也可以直接省略整個 `picture`。

## Utilities

- `has-background` 恢復元素的語義表面。
- `has-no-background` 移除填色、邊框、陰影與 backdrop filter。
- `has-no-padding` 移除內距。
- `is-rounded` 將圖片裁切成圓形。
- `is-spoiler` 在 hover 或 focus 前隱藏文字。
- `is-left-aligned`、`is-center-aligned`、`is-right-aligned` 與 `is-justify-aligned` 控制文字對齊。
- `is-<color>` 套用 Catppuccin 文字顏色。
- `is-bg-<color>` 套用 Catppuccin 背景顏色。

同時使用 `has-background` 與 `has-no-background` 時，以 `has-no-background` 為優先。

`is-rainbow` 提供一種帶點驕傲的硬邊彩虹文字樣式：

```html
<h1 class="is-rainbow">Rainbow heading</h1>
<p>Support <span class="is-rainbow">Pride</span>.</p>
```

請將它用在包含文字的元素，不要直接用在元件表面。

替視覺 spoiler 加上 `tabindex="0"`，讓鍵盤也能揭露內容：

```html
<span class="is-spoiler" tabindex="0">一個藏得很好的祕密</span>
```

## 無障礙

- 鍵盤 focus 保持清楚可見。
- 動畫遵守 `prefers-reduced-motion`。
- High contrast mode 會使用可讀的彩虹 fallback。
- 裝飾性 icon 使用 `aria-hidden="true"`。
- 原生 `details` 與 `summary` 不需要 JavaScript 就能折疊內容。
- 桌布屬於裝飾性圖片，因此使用空白 `alt`。

## 使用方式

1. 將 `index.html` 與 `assets` 目錄複製到網站根目錄。
2. 替換 demo 內容，或從 **Preset recipes** 複製需要的項目。
3. 更新連結、metadata、icon、圖片與照片來源。
4. 選擇材質，以及可選的 light／dark 桌布。
5. 編譯 SCSS。

使用 Dart Sass：

```sh
sass assets/css/links.scss assets/css/links.css
```

頁面使用相對資產路徑，可以部署在網域根目錄或 subpath 下。
