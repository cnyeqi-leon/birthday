three.js r158（本地 vendor）
==========================

文件：
  three.min.js        —— three.js r158 的 UMD 构建（MIT），页面到点后才按需加载
  THREE-LICENSE.txt   —— 官方 LICENSE（MIT）

来源：
  https://registry.npmmirror.com/three/0.158.0/files/build/three.min.js
  https://registry.npmmirror.com/three/0.158.0/files/LICENSE

为什么不用 CDN / ES Module：
  · 页面要能离线、无网络时可开（双击 index.html 也能有 3D）；
  · UMD 用 <script src> 动态注入，file:// 下也能加载；
  · 不写 importmap、不引任何外链。

说明：
  · 这是 r158 的 UMD 构建，浏览器控制台会打印一条官方弃用提示（console.warn，不是报错），
    因为 three.js 从 r150 起把 UMD 列为 deprecated；这不影响运行。
  · 想换版本：把新版本 three.min.js 覆盖到这里即可（需保持全局 THREE 的 UMD 构建）。
  · 加载失败 / 浏览器没有 WebGL 时，页面会自动回退到纯 CSS 气球。
