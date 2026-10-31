# Links

English | [繁體中文](./readme.zh-TW.md)

An ultra-lightweight, self-hosted landing page for your links.

- Semantic HTML
- SCSS and modern native CSS
- No JavaScript or frontend framework
- Catppuccin Latte and Mocha
- Solid, mica, and acrylic materials

## Demo

The project page is both a live component demo and a concise reference:

[Open the live demo](https://undecv.github.io/Links/)

Open **Preset recipes** near the bottom of the page for ready-made social and contact links.

## Structure

Every Links page uses the same three-level hierarchy:

```text
lk-container
└── lk-section
    └── lk-block-*
```

- `lk-container` is the single-column page frame.
- `lk-section` groups related content.
- `lk-block-*` is the actual UI component.

Keep the hierarchy flat. Place sections inside the container and blocks inside sections. Avoid nesting sections inside sections or independent blocks inside blocks.

Use native elements for their intended meaning. A section may be a `section`, `nav`, `address`, or `details`; a link block remains an `a` even when it looks like a button.

## Components

| Class | Purpose |
| --- | --- |
| `lk-block-header` | Profile, project, or page heading |
| `lk-block-link` | A prominent navigational link |
| `lk-block-text` | Headings, paragraphs, lists, and inline links |
| `lk-block-figure` | Static or clickable image content |
| `lk-block-footer` | Credits, copyright, and closing information |
| `lk-block-meta` | Compact secondary navigation |

`lk-block-icon` and `lk-block-content` are internal helpers, not additional hierarchy levels.

### Icons

A header or link icon slot accepts either one image or one Font Awesome icon:

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

Add `is-rounded` to circularly crop an image. Omit the icon slot entirely for a link without an icon.

### Figures

Wrap a `figure` in an anchor when the whole image should be clickable:

```html
<a class="lk-block-figure" href="https://example.com">
  <figure>
    <img src="./image.jpg" alt="A useful description" />
    <figcaption>An optional visible caption.</figcaption>
  </figure>
</a>
```

The anchor may be omitted for a static figure. `figcaption` is also optional when the image already has an appropriate `alt` description.

## Materials

Select a material on `body`:

```html
<body data-material="acrylic" data-wallpaper-effect="material">
```

| Value | Character |
| --- | --- |
| `solid` | Opaque semantic surfaces without backdrop blur |
| `mica` | Moderately transparent surfaces with a restrained blur |
| `acrylic` | More transparent surfaces with a stronger blur |

A missing or invalid `data-material` value falls back to solid.

Catppuccin Latte and Mocha follow `prefers-color-scheme`. There is no manual theme state.

## Wallpaper

Declare wallpaper images in HTML so relative paths remain relative to the document and continue to work under a subpath:

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

For photographic wallpapers, prefer responsive AVIF or WebP files using the
`image.<pixel-width>.<format>` naming pattern. Keep the original JPEG as a
fallback; PNG is usually larger for photographs.

`data-wallpaper-effect="material"` applies the active material's tint, blur, and saturation. Without it, the original wallpaper is shown.

Use the native `hidden` attribute to temporarily disable the wallpaper:

```html
<picture class="lk-wallpaper-source" hidden>...</picture>
```

You may also omit the `picture` entirely.

## Utilities

- `has-background` restores an element's semantic surface.
- `has-no-background` removes its fill, border, shadow, and backdrop filter.
- `has-no-padding` removes internal spacing.
- `is-rounded` circularly crops an image.
- `is-spoiler` visually hides text until it is hovered or focused.
- `is-left-aligned`, `is-center-aligned`, `is-right-aligned`, and `is-justify-aligned` control text alignment.
- `is-<color>` applies a Catppuccin text color.
- `is-bg-<color>` applies a Catppuccin background color.

If `has-background` and `has-no-background` are both present, `has-no-background` wins.

`is-rainbow` gives text a proud, hard-edged rainbow style:

```html
<h1 class="is-rainbow">Rainbow heading</h1>
<p>Support <span class="is-rainbow">Pride</span>.</p>
```

Apply it to a text-bearing element rather than a component surface.

Add `tabindex="0"` to make a visual spoiler keyboard-revealable:

```html
<span class="is-spoiler" tabindex="0">A well-kept secret</span>
```

## Accessibility

- Keyboard focus remains visible.
- Motion respects `prefers-reduced-motion`.
- High-contrast mode receives a readable rainbow fallback.
- Decorative icons use `aria-hidden="true"`.
- Native `details` and `summary` provide foldaway content without JavaScript.
- Wallpaper images use an empty alt value because they are decorative.

## Use

1. Copy `index.html` and the `assets` directory to your web root.
2. Replace the demo content or copy entries from **Preset recipes**.
3. Update links, metadata, icons, images, and photo credits.
4. Choose a material and optional light/dark wallpapers.
5. Compile the SCSS.

With Dart Sass:

```sh
sass assets/css/links.scss assets/css/links.css
```

The page uses relative asset paths and can be deployed at a domain root or under a subpath.

## License

MIT
