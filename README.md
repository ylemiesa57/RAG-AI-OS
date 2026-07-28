<img width="1363" height="786" alt="image" src="https://github.com/user-attachments/assets/85f7b709-c289-43c9-9544-019ebef3bccc" />


<h1 align="center">RAG-AI-OS</h1>

<p align="center">
  <strong>A LightRAG operating system built for verification.</strong><br />
  Local LLMs · multilingual embeddings · graph-aware retrieval · GPU-accelerated containers
</p>

<p align="center">
  <a href="https://ylemiesa57.github.io/RAG-AI-OS/"><img src="https://img.shields.io/badge/Docs_site-GitHub_Pages-2dd4a8?style=flat-square&labelColor=071210" alt="GitHub Pages" /></a>
  <a href="https://github.com/ylemiesa57/RAG-AI-OS"><img src="https://img.shields.io/badge/Stack-LightRAG_%2B_Kotaemon-f0b429?style=flat-square&labelColor=071210" alt="Stack" /></a>
  <a href="https://github.com/ylemiesa57/RAG-AI-OS"><img src="https://img.shields.io/badge/Runtime-CUDA_Podman-c5d4cf?style=flat-square&labelColor=071210" alt="Runtime" /></a>
</p>

<p align="center">
  <a href="https://ylemiesa57.github.io/RAG-AI-OS/">Project site</a>
  ·
  <a href="docs/QUICKSTART.md">Quick start</a>
  ·
  <a href="docs/LIGHTRAG_DOCS_GUIDE.md">Document management</a>
  ·
  <a href="docs/TESTING_PLAN.md">Testing plan</a>
</p>

---

## What it is

**RAG-AI-OS** is a containerized retrieval OS: LightRAG for graph-based knowledge, local llama.cpp inference, multilingual embeddings, and a Gradio UI—shaped for inspectable, verification-oriented workflows.

## Quick start

```bash
./launch_rag_app.sh
```

Open the UI at **http://localhost:7860** (or the port printed by the launcher).

For a matching local toolchain:

```bash
./setup_local_env.sh
```

## Capabilities

| Layer | What you get |
|-------|----------------|
| **Retrieval** | LightRAG graph indexing, entity-aware answers, folder-based sync |
| **Inference** | CUDA llama.cpp servers, GPU layer offload, tuned batch sizes |
| **Documents** | In-browser PDF.js viewing, selective upload/download of knowledge folders |
| **Ops** | Dynamic port discovery, container/local environment parity |

## Documentation

| Guide | Purpose |
|-------|---------|
| [Quick start](docs/QUICKSTART.md) | Launch, ports, GPU tuning, logs |
| [LightRAG docs guide](docs/LIGHTRAG_DOCS_GUIDE.md) | Folder sync, backup, restore |
| [Testing plan](docs/TESTING_PLAN.md) | Build, runtime, and integration checks |
| [Sample document](samples/sample_document.txt) | Content for indexing smoke tests |

## Project site

The GitHub Pages landing lives in [`docs/`](docs/) and publishes at:

**https://ylemiesa57.github.io/RAG-AI-OS/**

Enable it under **Settings → Pages → Deploy from a branch → `main` / `/docs`**.

## Monitoring

```bash
podman logs -f kotaemon-instance
podman exec kotaemon-instance tail -f /app/embedding_8802.log
podman exec kotaemon-instance tail -f /app/llm_8800.log
```

## License & status

Research / verification-oriented system. See repository activity for the latest container and model wiring.
