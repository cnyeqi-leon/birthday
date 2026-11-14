# 俞叶祺 · 17｜生日页

单文件静态生日邀请页，无 CDN、无后端、离线可开。

- 线上：https://cnyeqi-leon.github.io/birthday/
- 本地：直接双击 `index.html`，或 `python -m http.server 8820` 后访问 http://127.0.0.1:8820/

## 改内容

| 要改什么 | 在哪 |
| --- | --- |
| 生日日期 / 倒计时终点 | `index.html` 里 `CONFIG.birthday` |
| 时间、地点、邀请文案 | `CONFIG.event` |
| 关键词四则 | `CONFIG.keywords` |
| 照片格子标题 / 文件 | `CONFIG.photos` |
| 留言存储位置、祝福模板 | `CONFIG.guestbook` |
| 分享链接、标题 | `CONFIG.share` |
| 到点后的祝福文案 | `CONFIG.party.message` |
| 气球里的六句祝福 / 气球颜色 | `CONFIG.party.balloons` / `CONFIG.party.colors` |
| 页脚两句 | HTML 里的 `.end` / `.end-sub` |
| 主题色 | CSS `:root` 里的 `--accent`（深色）/ 浅色块里的 `--accent` |

## 换照片

把图片命名成 `1.jpg` … `6.jpg` 放进 `assets/photos/`，刷新即可，不用改代码。
没放图时显示渐变 + 线条图标占位。

## 到点之后（2026-11-19 00:00 起）

到点前保持冷色、没有气球；到点那一刻自动切暖色（深棕 + 琥珀，1.5s 过渡），同时：

- 页面边缘浮起 6 只纯 CSS 的 3D 气球（高光 / 绳线 / 缓慢浮动 + 摇摆 + 鼠标和滚动视差）
- 中央淡入祝福文字；点爆气球会炸出碎屑、并在中央显示那只气球里的祝福
- 气球全戳完后有「再放一次气球」按钮

想提前看效果：`index.html?now=1`（只影响预览，不改真实时间判断）。
`prefers-reduced-motion` 下所有动效关闭，只保留静态祝福文字。

## 深色 / 浅色

默认跟随系统 `prefers-color-scheme`，暖色状态也分别适配深色和浅色。
调试时可以强制：`index.html?theme=light`、`?theme=dark`（可与 `?now=1` 组合）。

## 部署

```bash
git add -A
git commit -m "update"
git push
```

GitHub Pages 从 `main` 分支根目录发布，push 后等 1–2 分钟生效。
