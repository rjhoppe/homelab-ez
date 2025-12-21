---
layout: default
title: ollama
---

# ollama

Run a local LLM on a Windows machine (with a GPU) that all your devices can access on the same, local network.

## 1: Install ollama on your Windows GPU machine

## 2: Get your host IPv4 address

Get the local IPv4 address for your Windows host (will need this later)
```
ipconfig
```
## 3: Set OLLAMA_HOST var for ollama local network access

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

## 4: Pull your ollama model of choice

On a mid range GPU (NVIDIA 3060), I choose to try the `qwen2.5-coder:7b-instruct`

You can pull and run in the same command, but I like doing them separately.

```
ollama pull qwen2.5-coder:7b-instruct
```

## 5: Run your ollama model
```
ollama run qwen2.5-coder:7b-instruct
```

To test your connection:

```
curl http://YOUR_WINDOWS_IP:11434/api/tags
```

To test with an API request (from an external device on the same network)
```
curl http://YOUR_WINDOWS_IP:11434/api/generate -d '{
  "model": "qwen2.5-coder:7b-instruct",
  "prompt": "Your coding prompt here",
  "stream": false
}'
```

## Other helpful ollama cmds

To see what model is loaded into ollama:
```
ollama ps
```

To stop running a model that is currently loaded into ollama
```
ollama stop <YOUR-MODEL-NAME-HERE>
```