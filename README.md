# 📦 我的文件仓库 (Cloudflare Worker 极简版)

这是一个基于 Cloudflare Workers、KV 和 D1 数据库构建的极简无服务器（Serverless）文件管理与托管系统。无需购买云服务器，利用 Cloudflare 的免费额度即可搭建一个属于你自己的私人文件仓库。

本教程为**纯网页端手动部署版**，无需安装环境，无需敲命令行代码，仅需在 Cloudflare 控制台点击即可完成部署！

## ✨ 特性

- **零成本部署**：完全基于 Cloudflare 的免费套餐（Workers + KV + D1）。
- **现代化 UI 面板**：全新设计的卡片式管理后台，完美适配电脑端与手机端（完美支持 iOS Safari 上传）。
- **安全鉴权**：采用自定义页面 + Cookie 验证，告别浏览器原生 Basic Auth 弹窗的简陋体验。
- **动态访问与下载**：
  - 预览/引用链接：`https://你的域名/widgets/文件名.扩展名`
  - 强制下载链接：`https://你的域名/download/文件名.扩展名`
- **全格式支持**：不限制上传文件类型，内置 MIME 路由推断，支持图片、文本、代码等多格式浏览器直接预览。

---

## 🛠 部署教程 (纯网页免命令行)

### 准备工作
注册并登录你的 [Cloudflare](https://dash.cloudflare.com/) 账号。

### 第一步：创建 KV 命名空间（用于存文件本体）
1. 在左侧菜单栏点击 **Workers & Pages** -> **KV**。
2. 点击右上角的 **Create a namespace**（创建命名空间）。
3. 名字随便填（例如填 `my_file_kv`），点击 **Add** 创建即可。

### 第二步：创建 D1 数据库（用于存文件列表信息）
1. 在左侧菜单栏点击 **Workers & Pages** -> **D1**。
2. 点击右上角的 **Create database**（创建数据库）。
3. 名字随便填（例如填 `file_db`），点击 **Create**。
4. 创建成功后，点击进入你刚创建的这个数据库。
5. 找到页面的 **Console**（控制台）选项卡。
6. 在输入框里粘贴下面这行 SQL 语句，然后点击 **Execute**（执行）按钮建表：
   ```sql
   CREATE TABLE IF NOT EXISTS files (filename TEXT PRIMARY KEY, size INTEGER, upload_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP);
