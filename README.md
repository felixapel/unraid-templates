# 🚀 felixapel's Unraid Community Applications Templates

[![Unraid](https://img.shields.io/badge/Unraid-Community%20Applications-blue?logo=unraid&logoColor=white)](https://unraid.net)
[![Release: 2.4.1](https://img.shields.io/badge/Release-2.4.1-0ea5e9.svg)](https://github.com/felixapel/book-translator-hub/releases/tag/v2.4.1)
[![Bookwarden Release: 1.3.2](https://img.shields.io/badge/Bookwarden-1.3.2-emerald.svg)](https://github.com/felixapel/calibre-bookwarden/releases/tag/v1.3.2)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![GitHub Sponsors](https://img.shields.io/badge/Sponsor-GitHub-ea4aaa?logo=github&logoColor=white)](https://github.com/sponsors/felixapel)
[![Ko-fi](https://img.shields.io/badge/Donate-Ko--fi-ff5e5b?logo=kofi&logoColor=white)](https://ko-fi.com/felixapel)

Official Unraid Community Applications repository maintained by [felixapel](https://github.com/felixapel).

---

## 📦 Available Templates

### 🌐 1. Book Translator Hub (`book-translator-hub.xml`)
> **Universal Bilingual Reading Overlay & Real-Time Translation Engine** for Calibre-Web Automated (CWA), Kavita, and self-hosted ebook libraries.

<p align="center">
  <img src="icons/book-translator-hub.png" alt="Book Translator Hub Icon" width="128" height="128">
</p>

* **Core Philosophy**: *"Read any book in any language with zero friction and literary elegance."*
* **Container Image**: `ghcr.io/felixapel/book-translator-hub:latest`
* **Web UI (Proxy Mode)**: `8385` (maps to container `8080`)
* **API Port**: `8390` (direct REST API & SSE streaming)
* **Project Repository**: [felixapel/book-translator-hub](https://github.com/felixapel/book-translator-hub)

#### Key Capabilities:
* **Real-Time Token Streaming (SSE)**: Streams translated words into the viewport as the LLM generates them via `/translate/stream` (~160ms time-to-first-token).
* **Instant Viewport Rush**: Concurrently translates paragraphs 1, 2, and 3 in parallel micro-batches directly to `/translate`.
* **Directional Lookahead Prefetch**: Pre-translates upcoming pages along the reader's directional trajectory for a 0ms page-turn experience.
* **High-Capacity IndexedDB Cache**: Client-side storage (`BookTranslatorDB`) overcomes browser 5MB `localStorage` limits, caching entire books offline.
* **Robust SQLite WAL Persistence**: Server-side cache with 256MB mmap and 64MB RAM page cache delivers sub-millisecond (<0.5ms) lookups.
* **Literary-Tuned Pipeline**: Preserves author tone, formatting, poetry, and character dialogue with sliding `[CONTEXT]` window.
* **Multi-Reader Native Support**: Seamlessly overlays on both **Calibre-Web Automated** (CWA) and **Kavita** EPUB reader views.
* **Auto-Detect & Dedicated E-Ink Mode**: Automatic source language detection and 1-bit high-contrast layout for e-readers.
* **Flexible LLM Backends**: Zero-cost local inference (**vLLM**, **Ollama**, **Bifrost**) or cloud providers (**Gemini**, **OpenAI**, **Claude**, **DeepSeek**, **Groq**).

#### Port & Path Mappings:
| Parameter | Type | Container Path / Target | Default Host Path / Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| **API Port** | Port | `8390` | `8390` | Direct translation REST API + SSE streaming |
| **Proxy Port** | Port | `8080` | `8385` | Injected reader proxy port (access your reader through this) |
| **Appdata Storage** | Path | `/app/data` | `/mnt/user/appdata/book-translator-hub/data` | SQLite translations cache database (requires container UID 101:GID 102 ownership, 0700) |
| **Runtime Role** | Env | `BT_ROLE` | `all` | `all` (combined API + proxy overlay), `api` (API only), `proxy` |
| **Calibre-Web URL** | Env | `CWA_URL` | *(Optional)* | Upstream URL for Calibre-Web (e.g. `http://192.168.0.122:8383`) |
| **Kavita URL** | Env | `KAVITA_URL` | *(Optional)* | Upstream URL for Kavita (e.g. `http://192.168.0.122:5547`) |
| **LLM Provider** | Env | `LLM_PROVIDER` | `local` | `local` (vLLM/Ollama), `gemini`, `openai`, `anthropic`, `groq`, `deepseek` |
| **LLM Model** | Env | `LLM_MODEL` | `gemma4-12b` | Target translation model name |
| **Local LLM URL** | Env | `BT_LOCAL_URL` | `http://192.168.0.122:2819/v1/chat/completions` | Local OpenAI-compatible API endpoint |
| **LLM API Key** | Env | `LLM_API_KEY` | *(Optional)* | API key for cloud providers |
| **Allowed Origins** | Env | `BT_ALLOWED_ORIGINS`| `*` | CORS allowed origins |
| **Auth Mode** | Env | `BT_AUTH_MODE` | `disabled` | Authentication mode: `disabled`, `token`, `cwa_session`, `reader_session` |
| **Allow Insecure Auth** | Env | `BT_ALLOW_INSECURE_AUTH` | `true` | Allows unauthenticated LAN operation when `BT_AUTH_MODE=disabled` |
| **Batch Size** | Env | `BT_BATCH_SIZE` | `6` | Max paragraphs per grouped LLM translation request |
| **Batch Source Budget** | Env | `BT_BATCH_SOURCE_TOKEN_BUDGET` | `1500` | Max input source tokens per batch group |
| **Prefetch Pacing Delay** | Env | `BT_CLIENT_PREFETCH_GAP_MS` | `1000` | Delay (ms) between lookahead prefetch calls |
| **Max Upstream Inflight** | Env | `BT_MAX_UPSTREAM_INFLIGHT` | `8` | Maximum concurrent requests dispatched to LLM backend |
| **Max Concurrent** | Env | `BT_MAX_CONCURRENT` | `8` | Maximum concurrent batch worker threads |
| **Request Timeout** | Env | `BT_TIMEOUT` | `90` | Upstream LLM HTTP timeout in seconds |
| **Context Window** | Env | `BT_CONTEXT_WINDOW` | `1` | Surrounding paragraphs provided as non-translated narrative context |
| **Timezone** | Env | `TZ` | `Europe/Berlin` | Container operational timezone |

---

### 🛡️ Calibre Bookwarden (reference only)
> Evidence-led Calibre metadata verification with a read-only Certificate A production profile.

<p align="center">
  <a href="https://github.com/felixapel/calibre-bookwarden/blob/main/docs/assets/certificate-a-overview.png"><img src="https://raw.githubusercontent.com/felixapel/calibre-bookwarden/main/docs/assets/certificate-a-overview.png" alt="Certificate A overview with synthetic example data" width="720"></a>
</p>

* **Approach**: Inspect attached formats, record provenance and uncertainty, and keep production library writes disabled.
* **Latest release**: [Calibre Bookwarden v1.3.2](https://github.com/felixapel/calibre-bookwarden/releases/tag/v1.3.2)
* **Project repository**: [felixapel/calibre-bookwarden](https://github.com/felixapel/calibre-bookwarden)
* **Supported deployment**: use the upstream [Certificate A installation guide](https://github.com/felixapel/calibre-bookwarden/blob/main/INSTALL.md) and [production operations runbook](https://github.com/felixapel/calibre-bookwarden/blob/main/docs/runbooks/production-operations.md).

This repository does not provide a validated Unraid one-click adapter for Bookwarden. The former XML is retained only as [`archive/calibre-bookwarden.xml.disabled`](archive/calibre-bookwarden.xml.disabled) for historical review and is not an installable template. Follow the upstream Certificate A Compose/runbook requirements, including their approval and isolation boundaries; do not assume a zero-risk or working single-container Unraid installation from this repository.

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
6. Navigate to the **Apps** (Community Applications) tab, or click **Add Container** and select **book-translator-hub** from the template dropdown.

---

## 💖 Support & Contributions

If you find these self-hosted reading and library management tools useful in your homelab:
* 🌟 Star the repositories on GitHub!
* ☕ [Support via Ko-fi](https://ko-fi.com/felixapel)
* 💖 [Sponsor on GitHub](https://github.com/sponsors/felixapel)

