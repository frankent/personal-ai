# 🧠 Personal AI with Ollama + Open WebUI

A self-hosted personal AI stack using [Ollama](https://ollama.com/) and [Open WebUI](https://github.com/open-webui/open-webui) that runs locally via Docker Compose. This setup lets you run large language models (LLMs) locally, with a clean browser-based interface.

---

## 📦 What is this?

This project provides a ready-to-run local AI assistant using:

| Component     | Description |
|---------------|-------------|
| **Ollama**     | A lightweight server to run LLMs locally with OpenAI-compatible API. |
| **Open WebUI** | A modern UI for chatting with models running via Ollama. |

This is ideal for privacy-conscious developers, tinkerers, or anyone who wants to run their own AI assistant on their machine without depending on cloud APIs.

---

## 🚀 How to Use

### 1. **Install Docker & Docker Compose**
Make sure Docker is installed on your system. You can get it from [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)

### 2. **Clone or Create the Project**

Place the following `docker-compose.yml` in your project folder:

```yaml
version: '3.8'

services:
  ollama:
    image: ollama/ollama
    container_name: ollama
    ports:
      - "11434:11434"
    volumes:
      - ./ollama_data:/root/.ollama
    restart: unless-stopped

  open-webui:
    image: ghcr.io/open-webui/open-webui:main
    container_name: open-webui
    ports:
      - "3000:3000"
    environment:
      - OLLAMA_BASE_URL=http://ollama:11434
    depends_on:
      - ollama
    volumes:
      - ./openwebui_data:/app/backend/data
    restart: unless-stopped
```

### 3. **Start the Services**

```bash
docker-compose up -d
```

This will download and run:
- Ollama server on port **11434**
- Open WebUI on port **3000**

### 4. **Access the Web Interface**

Open your browser and visit:  
👉 [http://localhost:3000](http://localhost:3000)

---

## 🧠 Using Ollama (CLI)

Once running, you can use `ollama` via Docker:

### Open a shell in the Ollama container:
```bash
docker exec -it ollama bash
```

### Inside the container, pull a model (e.g., LLaMA 3):
```bash
ollama pull llama3
```

To interact with the model:
```bash
ollama run llama3
```

Or from outside the container, via API or CLI if installed on host:
```bash
curl http://localhost:11434/api/generate -d '{
  "model": "llama3",
  "prompt": "Tell me a joke."
}'
```

---

## 📁 Volume Info

| Volume         | Purpose                         |
|----------------|----------------------------------|
| `./ollama_data`    | Caches downloaded models locally |
| `./openwebui_data` | Stores Web UI chat history/config |

You can back up or delete these folders safely.

---

## 📚 References

- 🔗 Ollama: https://ollama.com
- 🔗 Open WebUI: https://github.com/open-webui/open-webui

---

## 💡 Tips

- You can replace `llama3` with any other model supported by Ollama.
- Try models like `mistral`, `gemma`, or `codellama` depending on your hardware.
- Use the WebUI to manage chat, personas, and chat history more easily.

---

## 🛠️ Requirements

- Minimum: 8GB RAM (for small models)
- Recommended: 16GB+ RAM and GPU (NVIDIA) for better performance
- Works on Linux, macOS, Windows (WSL/Docker Desktop)

---

## 🧼 Stopping the stack

```bash
docker-compose down
```

---

## 🧩 Customization

You can add additional environment variables or ports to support advanced configurations. For example, adding authentication or switching to a different UI like Mistral WebUI.

---
