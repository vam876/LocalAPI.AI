# LocalAPI.AI
LocalAPI.ai 是一个开源的在线调用系统，专注于提供基于浏览器的 Ollama WEB UI客户端。该系统致力于为用户提供便捷、安全的 AI 服务体验，允许用户无需安装第三方软件即使用可进行智能对话、文本生成、模型管理等功能，并且支持在移动端（手机）进行远程使用。

在线体验： <a href="http://www.LocalAPI.ai">http://www.LocalAPI.ai</a>  

## 主要功能

• **远程调用**：支持通过 API 远程调用 Ollama 模型，实现云端计算资源的高效利用。

• **兼容移动端**：优化了浏览器的适配，用户可以在手机等移动设备上流畅地访问系统，进行远程模型调用和相关操作。

• **安全性增强**：提供身份验证和访问控制机制，确保模型调用的安全性。

• **多模型支持**：支持多种 Ollama 模型，满足不同场景下的需求。

• **代码高亮显示**：在交互界面中，代码片段会自动进行语法高亮显示，提升代码可读性和编写效率。


## 快速部署

### 方式一
可直接下载集成LocalAPI.ai的Nginx，修改nginx.conf配置文件中的的Ollama API地址既可以一键启动  <a href="https://github.com/vam876/LocalAPI.AI/releases/tag/Bete">https://github.com/vam876/LocalAPI.AI/releases/tag/Bete</a>  

### 方式二 
将以下文件部署到 web 服务即可快速访问：

```
│  index.html
│
└─assets
        index-6T7dvEla.js
        index-Bu-ZNHg4.css
```

### 使用 Nginx 反向代理（推荐）

利用 Nginx 的反向代理功能可以无需解决跨域限制，快速启动服务。以下是一个示例配置：

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

请将 `your_domain_or_ip` 替换为你的实际域名或IP地址。

## 常规方式：配置与使用

### 第一步：开启 Ollama 跨域支持

#### Windows

• **快速启动方式（临时变量）**：
  1. 通过右下角 "Ollama 图标" 或 "进程管理" 退出 Ollama 进程。
  2. 使用快捷键 `Windows + R` 打开 "运行" 对话框，或直接打开命令提示符界面。
  3. 执行 `set OLLAMA_ORIGINS=*`。
  4. 执行 `ollama serve`。
  5. 成功启动后，继续进行 "第二步骤：临时关闭浏览器跨域限制"。

• **常规配置方式**：
  1. 使用快捷键 `Windows + R` 打开 "运行" 对话框，输入 `sysdm.cpl`。
  2. 在 "系统属性" 中，点击 "环境变量"。
  3. 在 "用户变量" 中新建变量：
     ◦ 变量名：`OLLAMA_ORIGINS`
     ◦ 变量值：`*`
  4. 重启 Ollama 服务或计算机以生效。
  5. 如需远程访问 Ollama，设置 `OLLAMA_HOST` 环境变量为 `0.0.0.0`。

#### macOS

• **命令行设置（临时生效）**：
  1. 退出 Ollama 应用。
  2. 打开终端，输入以下命令：
     ```bash
     export OLLAMA_ORIGINS="*"
     ```
  3. 启动 Ollama 服务：
     ```bash
     ollama serve
     ```
  4. 成功启动后，继续进行 "第二步骤：临时关闭浏览器跨域限制"。

• **永久生效设置**：
  1. 编辑启动脚本（如 `.zshrc` 或 `.bash_profile`），添加上述 `export` 命令。
  2. 重新加载配置文件：
     ```bash
     source ~/.zshrc
     ```
  3. 重启 Ollama 应用以使配置生效。

#### Linux

• **命令行设置（临时生效）**：
  1. 退出 Ollama 应用。
  2. 打开终端，输入以下命令：
     ```bash
     export OLLAMA_ORIGINS="*"
     ```
  3. 启动 Ollama 服务：
     ```bash
     ollama serve
     ```
  4. 成功启动后，继续进行 "第二步骤：临时关闭浏览器跨域限制"。

• **永久生效设置**：
  1. 如果 Ollama 作为 `systemd` 服务运行，编辑服务配置文件：
     ```bash
     sudo systemctl edit ollama.service
     ```
  2. 在 `[Service]` 部分添加：
     ```ini
     Environment="OLLAMA_ORIGINS=*"

     
     ```
  3. 保存并退出。
  4. 重新加载 `systemd` 配置并重启 Ollama 服务：
     ```bash
     sudo systemctl daemon-reload

 ## 功能展示
 PC端展示
 ![微信截图_20250305182428](https://github.com/user-attachments/assets/f9af989f-3d4e-44b7-9647-a50d1379ccef)
 
移动端展示
 
 ![c88172f4065d071145927908aba19e4](https://github.com/user-attachments/assets/7bc596bd-f404-4157-ac2a-6dedf6f1fa54)

## 许可证

本项目采用 [Apache-2.0 许可证](LICENSE)。

## 联系方式

• **Email**：vamjun@Gmail.com
