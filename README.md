# 🚀 felixapel's Unraid Community Applications Templates

[![Unraid](https://img.shields.io/badge/Unraid-Community%20Applications-blue?logo=unraid&logoColor=white)](https://unraid.net)
[![Release: 2.4.1](https://img.shields.io/badge/Release-2.4.1-0ea5e9.svg)](https://github.com/felixapel/book-translator-hub/releases/tag/v2.4.2)
[![Bookwarden release](https://img.shields.io/github/v/release/felixapel/calibre-bookwarden?label=Bookwarden%20release&color=emerald)](https://github.com/felixapel/calibre-bookwarden/releases/latest)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![GitHub Sponsors](https://img.shields.io/badge/Sponsor-GitHub-ea4aaa?logo=github&logoColor=white)](https://github.com/sponsors/felixapel)
[![Ko-fi](https://img.shields.io/badge/Donate-Ko--fi-ff5e5b?logo=kofi&logoColor=white)](https://ko-fi.com/felixapel)

Official Unraid Community Applications repository maintained by [felixapel](https://github.com/felixapel).

---

## 📦 Available Templates

### 🌐 1. Book Translator Hub (`book-translator-hub.xml`)
> **Bilingual LLM translation overlay** for stock Calibre-Web Automated (CWA). Read CWA through the translator's proxy port and translate EPUB chapters in place with a local OpenAI-compatible LLM.

<p align="center">
  <img src="icons/book-translator-hub.png" alt="Book Translator Hub Icon" width="128" height="128">
</p>

* **Container image**: `ghcr.io/felixapel/cwa-ebook-translate-plugin:2.4.2@sha256:03bccc8524af49529281d61688f801423695622968b785090a022ab5d4b0b46c` (pinned by digest; no `latest`)
* **Release**: [v2.4.2](https://github.com/felixapel/book-translator-hub/releases/tag/v2.4.2)
* **Install guide**: [Community Applications profile](https://github.com/felixapel/book-translator-hub/blob/main/docs/install/community-applications.md)
* **Web UI (reader proxy)**: host `8385` → container `8080`. The API port `8390` is never published.

#### Certified profile
* Unraid 7.3.2 x86_64, stock CWA 4.x, local OpenAI-compatible LLM.
* One combined `BT_ROLE=all` container running as `101:102` with a read-only root filesystem, a private `/tmp`, all capabilities dropped and `no-new-privileges`.
* Readers authenticate with their existing CWA login (`BT_AUTH_MODE=cwa_session`). Keep CWA's *Allow Reverse Proxy Authentication* off.
* Kavita, Authentik forwarded identity, upgrades from v2.1.x and split roles use the source-built [`btctl`](https://github.com/felixapel/book-translator-hub/blob/main/docs/install/btctl.md) path instead.

#### Before the first start
```bash
mkdir -p /mnt/user/appdata/book-translator-hub/data
chown 101:102 /mnt/user/appdata/book-translator-hub/data
chmod 0700 /mnt/user/appdata/book-translator-hub/data
```

#### Template fields
| Parameter | Type | Target | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| **Appdata** | Path | `/app/data` | `/mnt/user/appdata/book-translator-hub/data` | Private translation cache (101:102, mode 0700) |
| **Reader Proxy Port** | Port | `8080` | `8385` | Open CWA through this port or route your reader domain to it |
| **CWA URL** | Env | `CWA_UPSTREAM` | *(required)* | e.g. `http://calibre-web-automated:8083` on the same custom network |
| **CWA Session Check URL** | Env | `BT_CWA_AUTH_URL` | *(required)* | CWA URL + `/ajax/emailstat` |
| **Public Reader Origin** | Env | `BT_PUBLIC_ORIGIN` | *(required)* | Exact browser origin, e.g. `https://books.example.com` |
| **LLM Model** | Env | `LLM_MODEL` | *(required)* | As listed by your endpoint's `/v1/models` |
| **Local LLM URL** | Env | `BT_LOCAL_URL` | *(required)* | Absolute `/v1/chat/completions` URL (host IP or container name) |
| **LLM Provider** | Env | `LLM_PROVIDER` | `local` | Certified: `local` |
| **Runtime / auth** | Env | `BT_ROLE`, `BT_AUTH_MODE`, `BT_BROWSER_AUTH_MODE`, `BT_BROWSER_CREDENTIALS` | `all`, `cwa_session`, `cwa_session`, `same-origin` | Fixed profile values; do not change |
| **Timezone** | Env | `TZ` | `Etc/UTC` | Container timezone |

---

### 🛡️ Calibre Bookwarden (reference only)
> Evidence-led Calibre metadata verification with a read-only Certificate A production profile.

<p align="center">
  <a href="https://github.com/felixapel/calibre-bookwarden/blob/main/docs/assets/certificate-a-overview.png"><img src="https://raw.githubusercontent.com/felixapel/calibre-bookwarden/main/docs/assets/certificate-a-overview.png" alt="Certificate A overview with synthetic example data" width="720"></a>
</p>

* **Approach**: Inspect attached formats, record provenance and uncertainty, and keep production library writes disabled.
* **Latest release**: [Calibre Bookwarden](https://github.com/felixapel/calibre-bookwarden/releases/latest)
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

