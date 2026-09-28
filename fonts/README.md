# fonts/

## Cubic_11.woff2

**Cubic 11**，像素字体，来自
[TakWolf/fusion-pixel-font](https://github.com/TakWolf/fusion-pixel-font)（2026.09.25）。

- 11px 点阵，用于本站正文字体（`--font-body`）。
- 授权：SIL Open Font License 1.1，见 `LICENSE.txt`。
- 页面用 `@font-face` 本地加载（`font-display: swap`），无外部 CDN 依赖。
- 覆盖：拉丁 95/96、CJK 基本区 9159 字、常用全角标点。
  **不含** `↗`（U+2197）—— 页面已不使用该符号。

## Minecraft-Ten.otf / Minecraft-Seven.otf

Minecraft 官方风格标题字体，取自
[Spectrollay-OreUI/OreUI](https://github.com/Spectrollay-OreUI/OreUI) 的 `src/assets/fonts/`
（MIT License, © 2020 Spectrollay），详见 `assets/README.md`。

- `Minecraft Ten`（590 字形）→ 页头条标题 `--font-title`
- `Minecraft Seven`（593 字形）→ 区块标题、下载项名称 `--font-head`
- **只含拉丁字形，不含 CJK**：中文靠 `Cubic 11` 兜底，
  所以这两个字体族在 CSS 里都写成 `'Minecraft Seven','Cubic 11',sans-serif`。
- 想换字体只需替换同名文件，无需改 CSS。
