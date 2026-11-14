# 俞叶祺 · 17｜上半年复盘

单页静态个人页：上半年复盘 + 成长照片 + 寄语签名 + 本机留言；到 2026-11-19 00:00 自动切暖色，出现 3D 气球和祝福。
零 CDN、无后端、可离线打开（双击 `index.html` 即可）。

- 线上：https://cnyeqi-leon.github.io/birthday/
- 本地：`python -m http.server 8820` 后打开 http://127.0.0.1:8820/

## 预览参数

| 参数 | 看到什么 |
| --- | --- |
| `?now=1&theme=dark` | **到点后最彩色效果**（暖色 + 8 只多色 3D 气球 + 礼花 + 祝福）；生日当天定时截图用的就是这个 |
| `?now=1&theme=light` | 到点后的浅色暖调版本 |
| `?theme=light` / `?theme=dark` | 只强制主题，不影响「到点前/后」 |
| 不带参数 | 按真实时间：11-19 00:00 前是冷色极光版，之后是暖色版 |

`?now=1` 只影响预览，不改真实时间判断。

## 改内容

| 要改什么 | 在哪 |
| --- | --- |
| 生日日期 / 倒计时终点 | `CONFIG.birthday` |
| 时间线（童年 / 少年 / 苗寨夜 / 现在） | `CONFIG.timeline`（`label` + `text`，现在都是「待填」） |
| 关键词 marquee | `CONFIG.keywords` |
| 照片文件 / 每格小字 | `CONFIG.photos`（`caption` 现在是「待填」） |
| 寄语文案 | HTML 里 `#note` 的 `.note-text` |
| 签名图 | `assets/signature/sign-white.png`（深色底）/ `sign-ink.png`（浅色底） |
| 留言存储键 | `CONFIG.guestbook.storageKey` |
| 分享标题 / 文案 / 链接 | `CONFIG.share` |
| 到点后的中央祝福 | `CONFIG.party.message` |
| 气球里的 8 条祝福 / 8 组气球颜色 | `CONFIG.party.balloons` / `CONFIG.party.colors`（每次进页面随机配对） |
| 3D 气球位置（桌面/手机） | JS 的 `LAYOUT_DESKTOP` / `LAYOUT_MOBILE` |
| 页脚收尾 | HTML 里的 `.end` / `.end-sub` |
| three.js 版本 | `assets/vendor/three.min.js`（见同目录 README.txt） |

## 照片

8 张真实照片按成长顺序放在 `assets/photos/1.jpg` … `8.jpg`，网页端已轻量压缩（原图备份在 `_原图备份/`，不进 git）。
点任意照片可看大图（灯箱，Esc / 点背景关闭）。每格下方的小字在 `CONFIG.photos[].caption` 里改。

## 到点之后（2026-11-19 00:00 起）

到点前：近黑底 + 多彩极光、kinetic 标题、时间线进度线、关键词 marquee、照片 3D 倾斜扫光、鼠标柔光、顶部滚动进度条；
**完全不加载 WebGL / three.js**（省电省流量）。

到点那一刻（1.5s 过渡到暖色深棕 + 琥珀）之后：

- 一次性礼花 + 彩带（canvas 粒子，约 3 秒自然消散，暖色为主 + 少量紫/青）
- 大号「17」出现时轻闪一次，中央祝福文字逐字亮起 + 光晕呼吸
- 背景换成暖色暗金尘 + 少量彩色光斑
- 8 只多色 **真 3D 气球**（three.js r158，本地 `assets/vendor/three.min.js`，懒加载）：
  车削成型 + 打结圆锥 + CatmullRom 细绳、MeshPhysicalMaterial（低 roughness + clearcoat）、
  暖色环境光/方向光/点光、环境贴图由本地 canvas 生成
- 缓慢上浮 + 正弦摇摆 + 鼠标/滚动视差 + 摄像机轻微推拉；点击射线拾取 → 爆开 + 碎屑 + 显示该气球祝福
- 每次进入页面随机打乱「颜色 ↔ 祝福」的配对；手机端气球 4 只
- 性能：`devicePixelRatio` 上限 2、`powerPreference: low-power`、页面隐藏时暂停 rAF；
  **没有 WebGL 或 three.js 加载失败时自动回退纯 CSS 气球**

`prefers-reduced-motion`：所有动画（极光、marquee、逐字、礼花、3D 循环等）关闭，只保留静态画面；点击仍可戳破气球。

## 部署

```bash
git add -A
git commit -m "update"
git push
```

GitHub Pages 从 `main` 分支根目录发布，push 后 30–90 秒生效。
