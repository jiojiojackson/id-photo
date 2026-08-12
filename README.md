# 📸 AI 智能证件照大师 (AI ID-Photo Master)

一个**100% 运行在浏览器本地**的高性能、极速、隐私安全的 AI 智能证件照制作工具。基于现代 WebAssembly 和 `@imgly/background-removal` 技术，无需上传照片到服务器，在您的电脑或手机（包括 Safari 和 Chrome）上直接完成自助拍照、自动智能抠图、一键换背景底色、智能美颜调色。

🏠 **在线演示 (Demo)**：[https://jiojiojackson.github.io/id-photo/](https://jiojiojackson.github.io/id-photo/)

---

## ✨ 核心特性

1. **📷 双端智能拍照与辅助线**
   - 支持电脑、手机（iPhone / Android）多摄像头调用，支持一键切换前置/后置摄像头。
   - 自动检测并针对前置摄像头应用**镜像画面（Mirror Effect）**，提供最自然舒服的自拍体验；后置摄像头自动取消镜像。
   - 包含专业人脸、眼睛、下巴及肩膀对齐**椭圆辅助线**，提示最佳拍摄姿势和人像距离。
   - 针对 iOS Safari / WebUI 做了深度体验优化，保证视频流流畅不卡顿。

2. **🪄 智能人像抠图 & 一键换底**
   - 采用先进的本地深度学习模型 (`@imgly/background-removal`)，在浏览器本地执行**毫米级**的高精度边缘分割，完美抠出每一缕发丝。
   - 支持一键换底：内置**白底**、**蓝底**、**红底**、**透明背景**，并提供**自定义调色盘**。
   - 抠图结果本地缓存，切换背景或调节参数时无需重复计算，实现**毫秒级实时渲染**。

3. **🎨 智能美颜与调色**
   - **一键美颜 (Auto Beautify)**：智能提升面部气色与亮度，平滑微调对比度和饱和度，呈现自然高级、不假白的面容。
   - **精细调色滑块**：亮度 (Brightness)、对比度 (Contrast)、饱和度 (Saturation)、曝光 (Exposure) 和色温冷暖 (Warmth)。

4. **💾 灵活导出**
   - 包含多种标准证件照尺寸预设：**一寸 (295x413px)**、**二寸 (413x579px)**、**护照/自定义 (390x567px)**。
   - 支持自定义导出格式：**JPEG (压缩体积)** 或 **PNG (无损画质)**。
   - 支持自定义文件大小/导出质量控制。

5. **🛡️ 极致的隐私安全**
   - 所有的图像处理、人脸剪裁、AI 抠图计算**全部在您的本地设备上完成**，没有网络上传步骤，100% 杜绝个人证件照信息泄露风险。

---

## 🛠️ 技术实现原理

### 1. 跨域隔离 (Cross-Origin Isolation) 的本地化加速
由于本应用需要在浏览器中以多线程方式高效运行 WebAssembly (WASM) 神经网络模型，必须使用浏览器底层的 `SharedArrayBuffer` API。而现代浏览器要求启用特定的 HTTP 安全标头：
- `Cross-Origin-Opener-Policy: same-origin`
- `Cross-Origin-Embedder-Policy: require-corp`

在 GitHub Pages 这种不支持自定义 HTTP 响应头的静态托管平台上，我们通过在前端集成 [coi-serviceworker](https://github.com/gzuidhof/coi-serviceworker) 成功拦截并动态模拟了这组安全标头，使 AI 抠图性能提升数倍，并彻底消除了跨域报错。

### 2. Canvas 像素滤镜引擎
结合 Canvas 复合操作，除了基本的 `ctx.filter` 亮度、对比度调节外，还引入了 `globalCompositeOperation = 'multiply'` 正片叠底色温混合技术，使得暖肤色和冷色调过渡极具自然质感。

---

## 🚀 部署指南 (GitHub Pages)

本项目已经通过 GitHub Actions 配置了全自动部署，当您将代码推送到 GitHub 时，将自动构建并上线：

1. **自动部署**：
   - 在本项目的 `.github/workflows/deploy.yml` 中已经为您配置好工作流。
   - 每次将代码 `push` 到 `main` 或者是 `master` 分支，GitHub Actions 就会自动将该静态网站发布到 GitHub Pages。

2. **手动部署步骤**：
   - 本项目为纯粹的静态单页面应用，您可以直接将 `index.html` 和 `coi-serviceworker.js` 放置在任意静态服务器中（如 Nginx, Apache, Vercel 等）。
   - 确保 `coi-serviceworker.js` 放置于网站根目录下，并在 `index.html` 的 `<head>` 顶部引入。

---

## 📱 移动端与 Safari 使用技巧

- **Safari 摄像头权限**：请确保在 iOS 的 `设置 -> Safari 浏览器 -> 相机` 中允许了访问权限。
- **首次抠图模型下载**：在首次点击“拍照”或“上传照片”进行 AI 抠图时，浏览器会自动在本地下载约几十MB的神经网络模型（仅需下载一次并自动缓存在浏览器中）。建议首次使用在 Wi-Fi 环境下运行，后续即可实现秒级离线处理。

---

## 📄 开源许可证

本项目基于 [MIT License](LICENSE) 许可证开源。所使用的核心抠图库技术归 `@imgly/background-removal` 团队所有。
