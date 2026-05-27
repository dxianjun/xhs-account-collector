# Vercel 部署指南

## 架构说明

```
┌─────────────────┐         ┌──────────────────────┐
│   Vercel (CDN)  │   API   │   你的服务器          │
│                 │◄────────│                      │
│  index.html     │ 请求    │  python web_service.py │
│  (管理页面)     │         │  (采集引擎)            │
└─────────────────┘         └──────────────────────┘
```

Vercel 部署的是纯静态前端页面，采集引擎仍需在你的服务器上运行。

## 部署步骤

### 1. 部署到 Vercel

方式一：**Vercel CLI**
```bash
# 安装 Vercel CLI
npm install -g vercel

# 进入部署目录
cd vercel-deploy

# 部署
vercel --prod
```

方式二：**Vercel Dashboard**
1. 打开 https://vercel.com/new
2. 上传 `vercel-deploy/` 文件夹
3. 点击 Deploy

### 2. 在服务器启动采集引擎

```bash
cd /path/to/xiaohongshu-account-collector/scripts
pip install fastapi uvicorn playwright
playwright install chromium
python web_service.py --host 0.0.0.0 --port 8000
```

### 3. 配置连接

部署完成后，在页面顶部的「后端服务配置」中填入你的服务器地址：
```
http://你的服务器IP:8000
```

> **注意**: 如果服务器有防火墙，需要开放 8000 端口。
> 建议使用 Nginx 反代 + HTTPS，或使用内网穿透工具（如 frp、ngrok）。

## 可选：Vercel Serverless API 代理

如果你不想暴露服务器端口，可以在 `api/` 下创建 Vercel Serverless 函数做代理转发。
需要 Node.js 环境，参考 `api/proxy.js`。
