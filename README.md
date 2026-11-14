# 俞叶祺 · 17｜上半年复盘

单页静态个人页：成长照片 7 格 + 寄语签名 + 留言（会传给寿星）+ 到点后的暖色彩蛋。
零 CDN、无后端代码（留言用外部小站 API）、可离线打开（除了留言同步）。

- 线上：https://cnyeqi-leon.github.io/birthday/
- 本地：`python -m http.server 8820` 后打开 http://127.0.0.1:8820/

## 预览参数

| 参数 | 看到什么 |
| --- | --- |
| `?now=1&theme=dark` | **到点后最彩色效果**（暖色 + 多色 3D 气球 + 礼花 + 祝福）；生日当天定时截图用的就是这个 |
| `?now=1&theme=light` | 到点后的浅色暖调版本 |
| `?theme=light` / `?theme=dark` | 只强制主题，不影响「到点前/后」 |
| `?inbox=1` | 只读留言收件箱（从服务器拉取，需要口令） |
| `?inbox=1&k=口令` | 直接带口令打开收件箱 |
| `?api=...` | 调试用：临时替换留言 API 地址 |
| 不带参数 | 按真实时间：11-19 00:00 前冷色极光版，之后暖色版 |

## 改内容

| 要改什么 | 在哪 |
| --- | --- |
| 生日日期 / 倒计时终点 | `CONFIG.birthday` |
| 时间线开关 | `CONFIG.showTimeline`（现在是 `false`，整块隐藏；改回 `true` 就出现） |
| 时间线文案 | `CONFIG.timeline`（4 条已填好，代码保留） |
| 关键词 marquee | `CONFIG.keywords` |
| 照片文件 / 每格小字 | `CONFIG.photos`（7 张，caption 已填） |
| 寄语文案 | HTML 里 `#note` 的 `.note-text` |
| 签名图 | `assets/signature/sign-white.png`（深色底）/ `sign-ink.png`（浅色底） |
| 留言后端地址 | `CONFIG.blessingApi` |
| 留言存储键 | `CONFIG.guestbook.storageKey` |
| 分享标题 / 文案 / 链接 | `CONFIG.share` |
| 到点后的中央祝福 | `CONFIG.party.message` |
| 气球里的 8 条祝福 / 8 组颜色 | `CONFIG.party.balloons` / `CONFIG.party.colors`（每次进页面随机配对） |
| 页脚收尾 | HTML 里的 `.end` / `.end-sub` |

## 照片

7 张真实照片按成长顺序放在 `assets/photos/1.jpg` … `7.jpg`，每格下面的小字已经填好：

1. 童年 · 瀑布旁边，笑得没心没肺
2. 游乐场里的小孩，手比耶
3. 躲在树叶后面的夏天
4. 一把折扇，装模作样
5. 苗寨的夜，第一次穿成这样
6. 提一盏灯，走过风雨桥
7. 喜欢的东西，就画下来

点照片看大图（Esc / 点背景关闭）。原图备份在 `_原图备份/`（不进 git）；网页端照片做过轻量压缩。

## 留言（会传给寿星）

- 提交留言时：先存进本机 `localStorage`，同时 `POST` 到 `CONFIG.blessingApi`：
  `https://junshi-pc.tailf36923.ts.net:10000/api/blessing`（body：`{"nick": "...", "text": "..."}`）
- 成功 → 提示「已送达，寿星能看到」，本机列表里标「已送达」；
  失败（没网 / 服务器没起来 / 跨域被拒）→ 留在本机，提示「已存在本机」。
- 服务器需要允许 `https://cnyeqi-leon.github.io` 的 CORS（现成后端已允许）。
- `?inbox=1`：只读收件箱。没有口令时会提示；口令在
  `D:\_军师\_生日会交流\token.txt`，可 `?inbox=1&k=口令` 直接打开（口令只在你本机，不写进页面）。

## 到点之后（2026-11-19 00:00 起）

到点前：近黑底 + 多彩极光、kinetic 标题、关键词 marquee、照片 3D 倾斜扫光、鼠标柔光、顶部滚动进度条；
**完全不加载 WebGL / three.js**。

到点那一刻（1.5s 过渡到暖色）之后：一次性礼花 + 彩带、大号「17」轻闪、逐字祝福 + 光晕呼吸、
暖色暗金尘背景、多色真 3D 气球（琥珀/珊瑚/蜜桃/奶油金/玫瑰/淡紫/青绿/薄荷，每次随机配对祝福）、
点击射线拾取爆开；桌面 8 只、手机 4 只；没有 WebGL 或 three.js 加载失败自动回退 CSS 气球。

`prefers-reduced-motion`：所有动画关闭，只保留静态画面；点击仍可戳破气球。

## 部署

```bash
git add -A
git commit -m "update"
git push
```

GitHub Pages 从 `main` 分支根目录发布，push 后 30–90 秒生效。
