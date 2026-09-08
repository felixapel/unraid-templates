# 🚀 felixapel's Unraid Community Applications Templates

[![Unraid](https://img.shields.io/badge/Unraid-Community%20Applications-blue?logo=unraid&logoColor=white)](https://unraid.net)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![GitHub Sponsors](https://img.shields.io/badge/Sponsor-GitHub-ea4aaa?logo=github&logoColor=white)](https://github.com/sponsors/felixapel)
[![Ko-fi](https://img.shields.io/badge/Donate-Ko--fi-ff5e5b?logo=kofi&logoColor=white)](https://ko-fi.com/felixapel)

Official Unraid Community Applications repository maintained by [felixapel](https://github.com/felixapel).

---

## 📦 Available Templates

### 🌐 1. Book Translator Hub (`book-translator-hub.xml`)
> **Universal Bilingual Reading Overlay & Translation Engine** for Calibre-Web Automated (CWA), Kavita, and self-hosted ebook libraries.

<p align="center">
  <img src="icons/book-translator-hub.png" alt="Book Translator Hub Icon" width="128" height="128">
</p>

* **Core Philosophy**: *"Read any book in any language with zero friction and literary elegance."*
* **Container Image**: `ghcr.io/felixapel/book-translator-hub:latest`
* **Web UI (Proxy Mode)**: `8385` (maps to container `8080`)
* **API Port**: `8390` (direct REST API & SSE streaming)
* **Project Repository**: [felixapel/book-translator-hub](https://github.com/felixapel/book-translator-hub)

#### Key Capabilities:
* **Zero-Wait Progressive Reveal**: Real-time Server-Sent Events (SSE) stream translated words as the LLM generates them.
* **Literary-Tuned Pipeline**: Preserves author tone, formatting, poetry, and character dialogue with sliding `[CONTEXT]` window.
* **Multi-Reader Native Support**: Seamlessly overlays on both **Calibre-Web Automated** (CWA) and **Kavita** EPUB reader views.
* **Flexible LLM Backends**: Zero-cost local inference (**vLLM**, **Ollama**, **Bifrost**) or cloud providers (**Gemini**, **OpenAI**, **Claude**, **DeepSeek**, **Groq**).
* **Robust SQLite WAL Persistence**: Instant hash-indexed translation lookup (<1ms) so revisited paragraphs load without LLM requests.

#### Port & Path Mappings:
| Parameter | Type | Container Path / Target | Default Host Path / Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| **API Port** | Port | `8390` | `8390` | Direct translation REST API + SSE streaming |
| **Proxy Port** | Port | `8080` | `8385` | Injected reader proxy port (access your reader through this) |
| **Appdata Storage** | Path | `/app/data` | `/mnt/user/appdata/book-translator-hub/data` | SQLite translations cache database |
| **Runtime Role** | Env | `BT_ROLE` | `api` | `api` (API only), `proxy` (reverse proxy overlay), or `all` |
| **Calibre-Web URL** | Env | `CWA_URL` | *(Optional)* | Upstream URL for Calibre-Web (e.g. `http://192.168.0.122:8383`) |
| **Kavita URL** | Env | `KAVITA_URL` | *(Optional)* | Upstream URL for Kavita (e.g. `http://192.168.0.122:5547`) |
| **LLM Provider** | Env | `LLM_PROVIDER` | `local` | `local` (vLLM/Ollama), `gemini`, `openai`, `anthropic`, `groq`, `deepseek` |
| **LLM Model** | Env | `LLM_MODEL` | `gemma4-12b` | Target translation model name |
| **Local LLM URL** | Env | `BT_LOCAL_URL` | `http://192.168.0.122:2819/v1/chat/completions` | Local OpenAI-compatible API endpoint |
| **LLM API Key** | Env | `LLM_API_KEY` | *(Optional)* | API key for cloud providers |
| **Allowed Origins** | Env | `BT_ALLOWED_ORIGINS`| `*` | CORS allowed origins |

---

### 🛡️ 2. Calibre Bookwarden (`calibre-bookwarden.xml`)
> **The Forensic Guardian for Calibre Libraries** — Content-grounded metadata verification, 360° deep audits, and high-fidelity cover triage.

<p align="center">
  <img src="icons/calibre-bookwarden.png" alt="Calibre Bookwarden Icon" width="128" height="128">
</p>

* **Core Philosophy**: *"The book file is the ground truth. LLMs are witnesses."*
* **Container Image**: `ghcr.io/felixapel/calibre-bookwarden:latest`
* **Web UI Port**: `8080` (maps to container `8080`)
* **Project Repository**: [felixapel/calibre-bookwarden](https://github.com/felixapel/calibre-bookwarden)

#### Key Capabilities:
* **Zero Hallucinations & Forensic Ground Truth**: Extracts exact metadata, title, author, and native covers directly from container files (`.epub`, `.pdf`, `.cbz`).
* **360° Forensic Audit Studio**: Deep inspection of author desyncs (`Author` vs `Author Sort`), broken paths, cover anomalies, and database hygiene.
* **Swipeable Cover Studio**: Side-by-side triage comparing current cover vs native embedded cover with Laplacian sharpness, Shannon entropy, and keyboard controls (<kbd>←</kbd> Skip, <kbd>→</kbd> Approve, <kbd>↑</kbd> Extract).
* **Curation Guardian**: **Series Gap Hunter** (detects missing book volumes) and **Duplicate Consolidator** (graphs cross-format duplicates).
* **Safety Invariant**: Operates in strict read-only mode by default (`BOOKWARDEN_READ_ONLY=true`) to guarantee zero risk of library corruption.

#### Default Port & Path Mappings:
| Parameter | Type | Container Path | Default Host Path / Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| **WebUI Port** | Port | `8080` | `8080` | WebUI and REST API dashboard |
| **Calibre Library** | Path | `/calibre` | `/mnt/user/data/media/books` | Root Calibre library containing `metadata.db` |
| **Config & Artifacts** | Path | `/config` | `/mnt/user/appdata/calibre-bookwarden` | Persistent audit reports, cache, and state |
| **Read Only Mode** | Env | `BOOKWARDEN_READ_ONLY` | `true` | When true, mutations are strictly blocked |
| **Gemini API Key** | Env | `GEMINI_API_KEY` | *(Optional)* | For multimodal cover verification |

---

## 🛠️ How to Add to Unraid

1. Open your **Unraid WebGUI**.
2. Navigate to the **Docker** tab.
3. Scroll down to the bottom and locate **Template Repositories**.
4. Paste the URL of this repository:
   ```text
   https://github.com/felixapel/unraid-templates
   ```
5. Click **Save**.
6. Navigate to the **Apps** (Community Applications) tab, or click **Add Container** and select **book-translator-hub** or **calibre-bookwarden** from the template dropdown.

---

## 💖 Support & Contributions

If you find these self-hosted reading and library management tools useful in your homelab:
* 🌟 Star the repositories on GitHub!
* ☕ [Support via Ko-fi](https://ko-fi.com/felixapel)
* 💖 [Sponsor on GitHub](https://github.com/sponsors/felixapel)
