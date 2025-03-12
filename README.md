
# LocalAPI.ai

![GitHub stars](https://img.shields.io/github/stars/vam876/LocalAPI.ai?style=social)
![GitHub forks](https://img.shields.io/github/forks/vam876/LocalAPI.ai?style=social)
![GitHub watchers](https://img.shields.io/github/watchers/vam876/LocalAPI.ai?style=social)
![GitHub repo size](https://img.shields.io/github/repo-size/vam876/LocalAPI.ai)
![GitHub language count](https://img.shields.io/github/languages/count/vam876/LocalAPI.ai)
![GitHub top language](https://img.shields.io/github/languages/top/vam876/LocalAPI.ai)
![GitHub last commit](https://img.shields.io/github/last-commit/vam876/LocalAPI.ai?color=red)
[![Discord](https://img.shields.io/badge/Discord-Join%20Chat-blue?logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---
[英文描述 / View in English](./README.en.md)

---
**LocalAPI.ai** 是一个基于浏览器的 Ollama 在线管理工具，专为追求极致简洁和高效而设计。**通过一个简单的 HTML 文件**，即可实现强大的 Ollama 客户端功能，支持智能聊天、代码高亮、推理折叠，以及丰富的模型管理和文本生成功能。无论何时何地，只需一个浏览器，即可开启 AI 交互之旅。

## ⭐ 项目亮点

- **轻量级设计**：仅需一个 HTML 文件即可部署，无需复杂安装或配置。
- **功能强大**：支持智能聊天、代码高亮、推理折叠，以及丰富的提示词库。
- **多模型支持**：兼容多种 Ollama 模型，满足不同场景需求。
- **跨平台支持**：兼容桌面端和移动端，随时随地访问。
- **隐私保护**：所有数据本地处理，无需上传，确保数据安全。

## 🦙 主要功能

- 📱 **浏览器管理 Ollama**：通过浏览器直接管理 Ollama，无需安装额外软件。
- 💡 **智能聊天与文本生成**：支持多语言对话和文本生成，提升创作效率。
- 🎨 **代码高亮与推理折叠**：代码片段自动高亮，推理过程可折叠，提升交互体验。
- 📚 **丰富的提示词库**：内置提示词库，激发创意，提升 AI 输出质量。
- 🔒 **隐私与安全**：数据本地处理，不上传云端，确保隐私安全。

## 🚀 快速体验

访问 [LocalAPI.ai](http://www.LocalAPI.ai) 或直接下载 [Ollama_WEB.html](https://github.com/vam876/LocalAPI.ai/releases) 文件，部署WEB服务打开即可开始使用。

##  🛠️ 部署指南

### ⚡⚡⚡极速部署方法
下载Ollama_WEB.html文件，通过火狐Firefox浏览器打开，直接访问/Ollama_WEB.html
（注意：需要参考（http://localapi.ai/tutorial） 开启Ollama跨域支持和临时关闭浏览器跨域限制）

| Filename           | MD5                               | SHA1                              |
|--------------------|-----------------------------------|-----------------------------------|
| Ollama_WEB.html    | 02a071719ae598b07910c768425b0c41   | 2fc83e2c232ad5ddb19e16530fb41b6f3390fcdc |


https://github.com/user-attachments/assets/d2f43f4a-3b5b-4e59-9a46-b65d12add90f



### ⚡⚡快速部署方法

#### 方式一：集成 Nginx

可直接下载集成 LocalAPI.ai 的 Nginx，修改 `nginx.conf` 配置文件中的 Ollama API 地址即可一键启动，无法解决跨域。

[下载地址](https://github.com/vam876/LocalAPI.AI/releases/tag/Bete)

#### 方式二：Web 服务部署

将以下文件部署到 Web 服务即可快速访问：

```
│  index.html
│
└─assets
        index-6T7dvEla.js
        index-Bu-ZNHg4.css
```



1. 下载 [Ollama_WEB.html](https://github.com/vam876/LocalAPI.ai/releases) 文件。
2. 使用支持 HTML 的浏览器（目前测试仅支持Firefox火狐浏览器）打开文件。
3. 或将文件部署到 Web 服务器，访问 `/Ollama_WEB.html`。

### ⚡高级部署

使用 Nginx 反向代理，快速启动服务，无需设置解决跨域限制。示例配置如下：

```nginx
events {
    worker_connections 1024;
}

http {
    include mime.types;
    default_type application/octet-stream;

    sendfile on;
    keepalive_timeout 65;

    # 禁用缓冲以支持流式响应
    proxy_buffering off;

    # 增大缓冲区设置，避免 502 Bad Gateway
    proxy_buffer_size 256k;
    proxy_buffers 4 256k;
    proxy_busy_buffers_size 512k;

    server {
        listen 80;  # 绑定80端口
        server_name your_domain_or_ip;  

        # 代理 Ollama 服务到 /api/
        location /api/ {
            proxy_pass http://127.0.0.1:11434/api/;  # 这里填写Ollama API服务的地址
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;

            # 处理 OPTIONS 请求
            if ($request_method = 'OPTIONS') {
                add_header 'Access-Control-Max-Age' 1728000;
                add_header 'Content-Type' 'text/plain charset=UTF-8';
                add_header 'Content-Length' 0;
                return 204;
            }
        }

        # 自定义 404 错误页面，重定向到首页
        error_page 404 /;
        location = / {
            internal;
        }

        # 可选：处理其他静态资源请求
        location / {
            try_files $uri $uri/ /;
        }
    }
}
```
---

## 功能展示

### PC 端展示

![PC端展示](https://github.com/user-attachments/assets/f9af989f-3d4e-44b7-9647-a50d1379ccef)

### 移动端展示

![移动端展示](https://github.com/user-attachments/assets/7bc596bd-f404-4157-ac2a-6dedf6f1fa54)

---
## 开发环境

Vite + React + TypeScript

## 贡献与支持

欢迎参与项目贡献！无论是功能改进、文档优化，还是提交 Issue，我们都期待您的参与。

- **GitHub 仓库**：[https://github.com/vam876/LocalAPI.ai](https://github.com/vam876/LocalAPI.ai)
- **联系方式**：vamjun@Gmail.com

## 许可证

本项目采用 [Apache-2.0 许可证](LICENSE)。


