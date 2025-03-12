

# LocalAPI.ai

![GitHub stars](https://img.shields.io/github/stars/vam876/LocalAPI.ai?style=social)
![GitHub forks](https://img.shields.io/github/forks/vam876/LocalAPI.ai?style=social)
![GitHub watchers](https://img.shields.io/github/watchers/vam876/LocalAPI.ai?style=social)
![GitHub repo size](https://img.shields.io/github/repo-size/vam876/LocalAPI.ai)
![GitHub language count](https://img.shields.io/github/languages/count/vam876/LocalAPI.ai)
![GitHub top language](https://img.shields.io/github/languages/top/vam876/LocalAPI.ai)
![GitHub last commit](https://img.shields.io/github/last-commit/vam876/LocalAPI.ai?color=red)
[![Discord](https://img.shields.io/badge/Discord-Join%20Chat-blue?logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

**LocalAPI.ai** is a browser-based Ollama management tool designed for simplicity and efficiency. **With just a single HTML file**, it delivers powerful Ollama client functionalities, including intelligent chat, code highlighting, collapsible reasoning, and robust model management and text generation capabilities. Whether you're on a desktop or mobile device, LocalAPI.ai brings AI interaction to your fingertips—anytime, anywhere.

## ⭐ Project Highlights

- **Lightweight Design**: Deploy with a single HTML file—no complex installation or configuration required.
- **Feature-Rich**: Supports intelligent chat, code highlighting, collapsible reasoning, and an extensive prompt library.
- **Multi-Model Support**: Compatible with various Ollama models to meet diverse needs.
- **Cross-Platform Support**: Works seamlessly on desktops and mobile devices.
- **Privacy Protection**: All data is processed locally, ensuring maximum security and privacy.

## 🦙 Main Features

- 📱 **Browser-Based Ollama Management**: Manage Ollama directly through your browser without additional software.
- 💡 **Intelligent Chat and Text Generation**: Supports multilingual conversations and text generation to enhance productivity.
- 🎨 **Code Highlighting and Collapsible Reasoning**: Automatically highlights code snippets and allows collapsible reasoning for a better user experience.
- 📚 **Extensive Prompt Library**: Built-in prompt library to inspire creativity and improve AI output quality.
- 🔒 **Privacy and Security**: Data is processed locally and never uploaded to the cloud, ensuring privacy and security.

## 🚀 Quick Experience

Visit [LocalAPI.ai](http://www.LocalAPI.ai) or download the [Ollama_WEB.html](https://github.com/vam876/LocalAPI.ai/releases) file to get started. Deploy it on a web server and open `/Ollama_WEB.html` to begin using.

## 🛠️ Deployment Guide

### ⚡⚡⚡ Ultra-Fast Deployment

1. Download the [Ollama_WEB.html](https://github.com/vam876/LocalAPI.ai/releases) file.
2. Open it with Firefox browser to access `/Ollama_WEB.html`.
3. Follow the tutorial at [http://localapi.ai/tutorial](http://localapi.ai/tutorial) to enable Ollama CORS support and temporarily disable browser CORS restrictions.

| Filename           | MD5                               | SHA1                              |
|--------------------|-----------------------------------|-----------------------------------|
| Ollama_WEB.html    | 02a071719ae598b07910c768425b0c41   | 2fc83e2c232ad5ddb19e16530fb41b6f3390fcdc |


https://github.com/user-attachments/assets/d2f43f4a-3b5b-4e59-9a46-b65d12add90f



### ⚡⚡ Fast Deployment Methods

#### Method 1: Integrated Nginx

Download the pre-configured Nginx with LocalAPI.ai and modify the `nginx.conf` to point to your Ollama API address for one-click startup. Note that this does not solve CORS issues.

[Download Here](https://github.com/vam876/LocalAPI.AI/releases/tag/Bete)

#### Method 2: Web Service Deployment

Deploy the following files to a web server for quick access:

```
│  index.html
│
└─assets
        index-6T7dvEla.js
        index-Bu-ZNHg4.css
```

### ⚡ Advanced Deployment

Use Nginx as a reverse proxy to quickly start the service without worrying about CORS restrictions. Example configuration:

```nginx
events {
    worker_connections 1024;
}

http {
    include mime.types;
    default_type application/octet-stream;

    sendfile on;
    keepalive_timeout 65;

    # Disable buffering to support streaming responses
    proxy_buffering off;

    # Increase buffer size to avoid 502 Bad Gateway
    proxy_buffer_size 256k;
    proxy_buffers 4 256k;
    proxy_busy_buffers_size 512k;

    server {
        listen 80;  # Bind to port 80
        server_name your_domain_or_ip;  

        # Proxy Ollama service to /api/
        location /api/ {
            proxy_pass http://127.0.0.1:11434/api/;  # Replace with your Ollama API address
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;

            # Handle OPTIONS requests
            if ($request_method = 'OPTIONS') {
                add_header 'Access-Control-Max-Age' 1728000;
                add_header 'Content-Type' 'text/plain charset=UTF-8';
                add_header 'Content-Length' 0;
                return 204;
            }
        }

        # Customize 404 error page to redirect to home
        error_page 404 /;
        location = / {
            internal;
        }

        # Optional: Handle other static resource requests
        location / {
            try_files $uri $uri/ /;
        }
    }
}
```

---

## Feature Showcase

### Desktop View

![Desktop View](https://github.com/user-attachments/assets/f9af989f-3d4e-44b7-9647-a50d1379ccef)

### Mobile View

![Mobile View](https://github.com/user-attachments/assets/7bc596bd-f404-4157-ac2a-6dedf6f1fa54)

---

## Development Environment

- **Frontend Framework**: Vite + React + TypeScript

## Contribution and Support

We welcome contributions! Whether it's feature improvements, documentation enhancements, or submitting issues, your participation is highly valued.

- **GitHub Repository**: [https://github.com/vam876/LocalAPI.ai](https://github.com/vam876/LocalAPI.ai)
- **Contact**: vamjun@Gmail.com

## License

This project is licensed under the [Apache-2.0 License](LICENSE).
