---
layout: default
title: ollama
---

# ollama

[Official Documentation](https://ollama.ai/docs)

Ollama is a powerful tool for running large language models locally. It allows you to set up and interact with various LLMs on your machine, making them accessible across your local network.

## Installation

### Install Ollama on your Windows GPU machine

## Configuration for Network Access

### Get your host IPv4 address

Get the local IPv4 address for your Windows host (will need this later)
```
ipconfig
```
### Set OLLAMA_HOST Environment Variable

Set some env vars for ollama to allow other machines to access it on the same network

* Press Win + R, type sysdm.cpl, hit Enter
* Go to "Advanced" tab → "Environment Variables"
* Under "User variables" (top section), click "New"
* Variable name: OLLAMA_HOST
* Variable value: 0.0.0.0:11434
* Click OK on all dialogs

Or set the env via PowerShell
```
set OLLAMA_HOST=0.0.0.0:11434
```

Restart ollama after setting this var

## Model Management

### Pull your Ollama model of choice

On a mid range GPU (NVIDIA 3060), I choose to try the `qwen2.5-coder:7b-instruct`

You can pull and run in the same command, but I like doing them separately.

```
ollama pull qwen2.5-coder:7b-instruct
```

### Run your Ollama model
```
ollama run qwen2.5-coder:7b-instruct
```

## Testing Connection and API

### Test your connection

```
curl http://YOUR_WINDOWS_IP:11434/api/tags
```

### Test with an API request

```
curl http://YOUR_WINDOWS_IP:11434/api/generate -d '{
  "model": "qwen2.5-coder:7b-instruct",
  "prompt": "Your coding prompt here",
  "stream": false
}'
```

## Other Helpful Ollama Commands

To see what model is loaded into ollama:
```
ollama ps
```

To stop running a model that is currently loaded into ollama
```
ollama stop <YOUR-MODEL-NAME-HERE>
```