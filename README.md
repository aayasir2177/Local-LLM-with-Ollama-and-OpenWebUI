# Ollama + Open WebUI

This lab documents the installation, configuration, and troubleshooting process of an AI stack built with Ollama and Open WebUI.

## Install Ollama

Installed Ollama using the official installation script provided by the Ollama.

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

## Download and Install a Model

Downloaded and launched the llama 3.2. If the model is not already present, Ollama automatically downloads it.

```bash
ollama run llama3.2
```

## Verify Installation

```bash
ollama list

curl http://localhost:11434/api/tags
```

## Install Open WebUI

Open WebUI was deployed inside a Docker container and configured to communicate with the Ollama server running on the host system.

### Run Open WebUI

```bash
docker run -d \
  -p 3000:8080 \
  --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:main
```

### Verify the Container

```bash
docker ps
```

### View Container Logs

```bash
docker logs open-webui
```

## Configure Ollama

By default, Ollama only listens on `127.0.0.1`, which prevents Docker containers from reaching it. I bind it to all network interfaces.

```bash
OLLAMA_HOST=0.0.0.0:11434 ollama serve
```

Verify that Ollama is listening:

```bash
ss -tlnp | grep 11434
```

## Test Connectivity from Open WebUI

Verify that the container can resolve the host and reach the Ollama API.

```bash
docker exec open-webui ping -c 1 host.docker.internal

docker exec open-webui sh -c "curl http://host.docker.internal:11434/api/tags"
```

## Access Open WebUI

Open a web browser and navigate to:

```
http://localhost:3000
```

## What I Learned

* Download and run local LLMs with Ollama
* Start and manage the Ollama server manually
* Use the Ollama REST API to verify available models
* Expose Ollama on `0.0.0.0` for container access
* Deploy Open WebUI as a Docker container
* Use Docker volumes for persistent application data
* Troubleshoot networking issues between Docker and host services

## Screenshots

<img width="1600" height="900" alt="1" src="https://github.com/user-attachments/assets/3f303408-d1c3-4d0e-b666-e3dfb8523519" />

<img width="1600" height="900" alt="2" src="https://github.com/user-attachments/assets/827ee666-01ec-45fa-bb76-7dd7201cac79" />

