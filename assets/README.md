# assets/

UI 素材取自 **OreUI 设计**（[Spectrollay-OreUI/OreUI](https://github.com/Spectrollay-OreUI/OreUI)，
MIT License, © 2020 Spectrollay），仅作本站视觉参考与复用，未修改文件内容。

本项目是**纯静态页、无构建链**，因此没有引入 OreUI 的 Vite 组件体系
（`oreui-button` / `oreui-show-block` 等自定义元素），而是：

1. 沿用其**设计令牌**（`src/components/design/colors/style.css`）：灰阶
   `#1E1E1F → #FFFFFF`、品牌绿 `#3C8527`、状态色 `#2E6BE5 / #D3791F / #CA3636`；
2. 沿用其**视觉规范**：2px 实心边框 + 内浮雕高光
   （`inset 3px 3px rgba(255,255,255,.6)`、`inset -3px -7px rgba(255,255,255,.4)`）、
   `box-shadow: inset 0 -4px` 的底部阴影、按钮按下时高度 4px 的位移；
3. 复用其**像素图标 PNG**。

## icons/

来自 `src/assets/images/`，均为像素风格、带 alpha 通道的白色图标。
**只保留页面实际引用的 3 个**，其余（`check` / `copy` / `cross` / `info` 等）已删除：

| 文件 | 原尺寸 | 用途 |
|---|---|---|
| `arrowDown_white.png` | 60×32 | 整合包下载项图标 |
| `arrowRight_white.png` | 32×60 | 降级态（指向 Releases）图标 |
| `ExternalLink_white.png` | 45×45 | 启动器外链项图标 |

> 图标**非正方形**，CSS 里一律按 `height` 定尺、`width:auto`，避免拉伸变形；
> 同时加 `image-rendering:pixelated` 保持像素锐利。

另有 `minecraft_logo.png`（300×300，调色板 PNG）：**站点图标（favicon / apple-touch-icon）**，
取自 [minecraft.net](https://www.minecraft.net/) 官方素材
`Homepage_Gameplay-Trailer_MC-OV-logo_300x300.png`——非 OreUI 素材、未修改内容，
仅用于浏览器标签页与移动端主屏图标。

## 字体

`fonts/` 下的 Minecraft 系字体（`Minecraft-Ten.otf`、`Minecraft-Seven.otf`）
同样来自上述仓库的 `src/assets/fonts/`。它们**只含拉丁字形（各约 590 个）**，
不含 CJK，所以中文由本站自托管的 Cubic 11 兜底 —— 详见 `fonts/README.md`。

## 未引入的文件

未下载 OreUI 仓库里的 `NotoSans*.ttf`（单个 5 MB+）、`Loading.gif` 与插画 PNG：
页面用不到，避免无谓的仓库体积。
