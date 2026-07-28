# RAG-AI-OS Documentation

Project guides for the containerized LightRAG system, plus the public GitHub Pages site.

**Site:** [ylemiesa57.github.io/RAG-AI-OS](https://ylemiesa57.github.io/RAG-AI-OS/) (`index.html`)

| Document | Description |
|----------|-------------|
| [Quick Start](QUICKSTART.md) | Launch, ports, GPU tuning, and day-to-day operations |
| [LightRAG Document Management](LIGHTRAG_DOCS_GUIDE.md) | Folder-based document sync, backup, and restore |
| [Testing Plan](TESTING_PLAN.md) | Container build, runtime, and integration test plan |

Sample content for indexing tests lives in [`../samples/sample_document.txt`](../samples/sample_document.txt).

## GitHub Pages

Publish from the `docs/` folder on `main`:

1. Repository **Settings → Pages**
2. **Deploy from a branch**
3. Branch: `main`, folder: `/docs`
4. Save — site appears at `https://ylemiesa57.github.io/RAG-AI-OS/`
