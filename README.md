# BPB Easy Active Config MAIN v9.0.0

> **Built by [mcoders](https://github.com/mcodersir)**
>
> [📖 مستندات فارسی (Farsi Documentation)](./README_FA.md)

BPB Easy Active Config MAIN is an all-in-one, offline-first desktop tool that guides users through the entire process of deploying a Cloudflare Worker, scanning for quality IP endpoints, and generating tested, working proxy configurations — all from a single, self-contained local application with a beautiful Persian (Farsi) web interface.

---

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [System Requirements](#system-requirements)
- [Quick Start](#quick-start)
- [Project Structure](#project-structure)
- [Step-by-Step User Guide](#step-by-step-user-guide)
- [CLI Usage](#cli-usage)
- [API Reference](#api-reference)
- [Code Protection](#code-protection)
- [Integrated Sources](#integrated-sources)
- [Output Files](#output-files)
- [Troubleshooting](#troubleshooting)
- [Security Notice](#security-notice)
- [License](#license)

---

## Overview

BPB Easy Active Config MAIN v9 is designed for users who want a simple, guided experience to:

1. **Create a Cloudflare account** and deploy a VLESS-over-WebSocket worker
2. **Scan Cloudflare IP endpoints** for quality and low latency
3. **Fetch, test, and generate proxy configurations** that actually work
4. **Export the best configuration** ready to use in any compatible client

The application runs entirely locally on the user's machine using a built-in HTTP server. The UI is fully Persian (Farsi), RTL, responsive, and requires no external CDN — all CSS, JS, and icons are bundled within the project.

### What Makes v9 Different

Version 9 introduces several major improvements over previous versions:

- **Real VLESS-over-WebSocket probing**: Instead of relying solely on TCP/TLS connectivity tests, v9 performs an actual WebSocket upgrade followed by a lightweight VLESS protocol probe. This produces results that are significantly closer to real-world usability, reducing false positives where an endpoint responds to TCP but fails to actually proxy traffic through the VLESS protocol.

- **Integrated Cloudflare Deploy Assistant**: The deploy step is now fully integrated into the wizard. Users no longer need to manually upload worker files — the application uses the Cloudflare API with multipart/form-data to deploy the bundled worker.js directly to the user's own account.

- **Smart Auto Mode**: The testing engine tries original BPB subscription configurations first. Only if none of them work does it automatically fall back to generating modified configurations using Clean IP endpoints, saving time for users whose original configs are already functional.

- **No External Dependencies**: The Python backend requires only the standard library — no pip install needed. All JavaScript, CSS, and worker code is bundled locally.

---

## Key Features

| Feature | Description |
|---------|-------------|
| **Step-by-Step Wizard** | 6 clear steps from start to final config output |
| **Persian (Farsi) UI** | Fully RTL interface with Persian labels and instructions |
| **Responsive Design** | Works on desktop, tablet, and mobile browsers |
| **No CDN Required** | All assets are local — works completely offline |
| **Cloudflare Deploy** | Deploy worker.js directly via Cloudflare API |
| **IP Quality Scanner** | Scan Cloudflare IP ranges for low-latency endpoints |
| **Smart Config Testing** | Auto mode tests base configs first, then falls back to Clean IP |
| **VLESS WS Probe** | Real WebSocket upgrade + VLESS protocol validation |
| **CLI Mode** | Command-line interface for automation and scripting |
| **Open Source** | Full readable source code — no obfuscation, fully auditable |

---

## System Requirements

- **Python**: 3.9 or higher (no external packages required)
- **OS**: Windows 10+, macOS 11+, or Linux (Ubuntu 20.04+)
- **Browser**: Any modern browser (Chrome, Firefox, Edge, Safari)
- **Network**: Internet access for Cloudflare API and subscription fetching
- **Disk Space**: ~50 MB for the application and output files

---

## Quick Start

### Windows

Double-click the batch file:

```bat
run_windows.bat
```

Or manually:

```bat
python start.py
```

### macOS / Linux

```bash
chmod +x run_mac_linux.sh
./run_mac_linux.sh
```

Or manually:

```bash
python3 start.py
```

The application will automatically:
1. Find a free local port (starting from 8765)
2. Start the built-in HTTP server
3. Open your default browser to the wizard UI

---

## Project Structure

```
BPB_Easy_Active_Config_MAIN_v9/
├── start.py                          # Main entry point - launches local HTTP server
├── cli.py                            # CLI interface for headless/automated usage
├── run_windows.bat                   # Windows launcher script
├── run_mac_linux.sh                  # macOS/Linux launcher script
├── requirements.txt                  # No external dependencies needed
├── VERSION.txt                       # Version identifier
├── LICENSE                           # MIT License with usage conditions
├── README.md                         # This documentation
├── README_FA.md                      # Farsi documentation
│
├── src/                              # Core Python modules (readable source)
│   ├── __init__.py
│   ├── core.py                       # Subscription parsing, config testing, IP scanning
│   └── cloudflare_deployer.py        # Cloudflare API integration
│
├── ui/                               # Web interface
│   ├── index.html                    # Main HTML - 6-step wizard layout
│   ├── styles.css                    # Full responsive CSS with Vazirmatn font
│   └── app.js                        # Frontend JavaScript logic
│
├── integrated_sources/               # Bundled source projects
│   ├── README_FA.md
│   ├── BPB_Worker_Panel_Bundle/
│   │   ├── worker.js                 # Cloudflare Worker script
│   │   └── README_FA.md
│   ├── BPB_Wizard_Internal/
│   │   ├── local_wizard_flow.py      # Wizard state management
│   │   └── README_FA.md
│   ├── SenPaiScanner_Core/
│   │   ├── senpai_scanner_core.py    # IP scanner re-exports
│   │   └── README_FA.md
│   └── Rasoul_Config_Modifier/
│       ├── config_modifier_core.py   # Config modifier re-exports
│       └── README_FA.md
│
├── examples/                         # Sample input files
│   ├── sample_ips.txt
│   └── sample_subscription.txt
│
├── output/                           # Generated output directory
│   └── .gitkeep
│
└── docs/                             # Additional Farsi documentation
    ├── FAQ_FA.md
    ├── SAFE_USE_FA.md
    ├── SOURCES_FA.md
    ├── DEPLOY_ASSISTANT_FA.md
    ├── TROUBLESHOOTING_FA.md
    └── NO_CDN_FA.md
```

---

## Step-by-Step User Guide

### Step 1: Welcome & Prerequisites

When you first launch the application, you'll see the welcome screen. Before proceeding, ensure:

- You have an accessible email address (if your team provides an Atomic Mail address, use that specific one)
- You understand that this Worker and its configurations are for your own authorized account and usage only

### Step 2: Email & Cloudflare Setup

1. If your team provided an Atomic Mail email, use that exact address
2. If you need a new email, click the **Atomic Mail** button to create one
3. Click **Cloudflare Sign Up** to create a Cloudflare account
4. Use the same email for both Cloudflare and your project email
5. Once you're in the Cloudflare Dashboard, proceed to the next step

### Step 3: Deploy Worker

This step deploys the bundled BPB worker.js directly to your Cloudflare account:

1. **Get your API Token**: Click **API Tokens** → My Profile → API Tokens → Create Token
2. Set permissions to: **Account → Workers Scripts → Edit**
3. Copy the token and paste it into the **Cloudflare API Token** field
4. Click **"Test Token & Find Account ID"** — this automatically populates your Account ID
5. Generate a UUID by clicking **"Generate Secure UUID"**
6. Click **"Deploy Internal File"** to upload the worker to your account

**Common Error**: If the error path contains `accounts/cfat_...`, it means you've pasted the API Token into the Account ID field. Always use the "Test Token" button first.

### Step 4: IP Quality Scanner (Optional)

This step is optional but recommended when base configurations don't work well:

1. Enter IP addresses or CIDR ranges to scan, or use random Cloudflare IP generation
2. Select which ports to test (443, 8443, 2053, etc.)
3. Click **"Scan Quality & Save"** to test endpoints
4. After scanning, click **"Use Clean IPs in Config Test"** to transfer results to the next step

The scanner performs real HTTP/TLS probes against Cloudflare's CDN endpoints and scores them based on connectivity and latency.

### Step 5: Subscription & Config Testing

1. **Paste your Subscription URL** — this is typically the worker URL from Step 3 (e.g., `https://worker-name.account.workers.dev/sub`)
2. Click **"Check Link"** to verify the subscription is accessible
3. Choose a testing mode:
   - **Smart Mode** (recommended): Tests original BPB configs first, falls back to Clean IP if needed
   - **Base Only**: Only tests the original subscription configurations
   - **Clean IP Only**: Only generates configs using scanned/provided clean IPs
4. Click **"Start Execution & Build Testable Output"**

### Step 6: Final Output

- The best configuration is automatically selected and displayed
- Click **"Copy Config"** to copy it to clipboard
- All working configurations are saved to `output/working_configs.txt`
- The best configuration is saved to `output/best_active_config.txt`

---

## CLI Usage

For users who prefer command-line or need automation, the CLI provides two main commands:

### IP Scanning

```bash
python3 cli.py scan-ips \
  --ips ips.txt \
  --cidrs cidrs.txt \
  --random 80 \
  --ports 443,8443,2053,2083,2087,2096 \
  --sni speed.cloudflare.com \
  --timeout 5 \
  --workers 48 \
  --limit 800
```

**Parameters:**

| Parameter | Default | Description |
|-----------|---------|-------------|
| `--ips` | "" | File containing IP addresses, one per line |
| `--cidrs` | "" | File containing CIDR ranges, one per line |
| `--random` | 80 | Number of random Cloudflare IPs to generate |
| `--ports` | 443,8443,2053,2083,2087,2096 | Comma-separated ports to test |
| `--sni` | speed.cloudflare.com | SNI hostname for TLS probes |
| `--timeout` | 5 | Connection timeout in seconds |
| `--workers` | 48 | Number of concurrent test workers |
| `--limit` | 800 | Maximum number of endpoints to scan |

### Config Testing

```bash
python3 cli.py run \
  --sub "https://worker-name.account.workers.dev/sub" \
  --mode auto \
  --random 240 \
  --ports 443 \
  --timeout 6 \
  --workers 32 \
  --limit 1500
```

**Parameters:**

| Parameter | Default | Description |
|-----------|---------|-------------|
| `--sub` | (required) | BPB subscription URL |
| `--mode` | auto | Testing mode: auto, base, or clean_ip |
| `--ips` | "" | Clean IP file for clean_ip mode |
| `--random` | 240 | Random Cloudflare IPs for auto/clean_ip mode |
| `--ports` | 443 | Comma-separated ports for generated configs |
| `--timeout` | 6 | Connection timeout per test in seconds |
| `--workers` | 32 | Number of concurrent test workers |
| `--limit` | 1500 | Maximum number of configs to test |

---

## API Reference

The local HTTP server exposes the following API endpoints:

### GET /api/status

Returns application status and configuration.

```json
{
  "app": "BPB Easy Active Config MAIN v9.0",
  "brand": "ساخته شده توسط mcoders",
  "no_cdn": true,
  "output_dir": "/path/to/output",
  "bundled_worker_present": true,
  "all_ports": [443, 8443, 2053, ...],
  "links": { ... }
}
```

### POST /api/fetch

Fetches and parses a subscription URL.

**Request Body:**
```json
{
  "subscription_url": "https://worker.workers.dev/sub",
  "timeout": 18
}
```

**Response:**
```json
{
  "ok": true,
  "total_lines": 15,
  "supported_configs": 12,
  "examples": ["VLESS worker.workers.dev:443", ...],
  "saved": "/path/to/output/base_configs.txt"
}
```

### POST /api/scan-ips

Scans IP endpoints for quality and latency.

**Request Body:**
```json
{
  "ip_text": "104.16.72.162\n172.64.100.8",
  "cidr_text": "104.16.0.0/24",
  "random_count": 160,
  "ip_limit": 900,
  "timeout": 4,
  "workers": 64,
  "sni_host": "speed.cloudflare.com",
  "ports": [443, 8443]
}
```

### POST /api/run

Main execution endpoint — fetches subscription, generates configs, and tests them.

**Request Body:**
```json
{
  "subscription_url": "https://worker.workers.dev/sub",
  "mode": "auto",
  "timeout": 6,
  "workers": 32,
  "limit": 1600,
  "random_count": 240,
  "ip_list": "",
  "ports": [443, 8443]
}
```

### POST /api/cf-verify

Verifies a Cloudflare API token and lists associated accounts.

**Request Body:**
```json
{
  "api_token": "your-cloudflare-api-token"
}
```

### POST /api/cf-deploy

Deploys the bundled worker.js to Cloudflare.

**Request Body:**
```json
{
  "api_token": "your-cloudflare-api-token",
  "account_id": "32-char-hex-account-id",
  "worker_name": "bpb-panel",
  "uuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "sub_path": "sub",
  "proxy_ip": ""
}
```

### GET /api/deploy-config

Returns saved deploy configuration (API token is masked).

```json
{
  "ok": true,
  "config": {
    "api_token_masked": "********abcd",
    "account_id": "32-char-hex",
    "worker_name": "bpb-panel",
    "uuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "sub_path": "sub",
    "proxy_ip": ""
  }
}
```

### POST /api/open-url

Opens a safe, whitelisted URL in the user's default browser.

### GET /api/open-output

Opens the output directory in the system file manager.

### GET /api/open-integrated-folder

Opens the integrated sources directory.

---

## What's New in v1.5

### IP Scanner Improvements (SenPaiScanner Methodology)
The IP scanner has been completely overhauled with techniques from [SenPaiScanner](https://github.com/MatinSenPai/SenPaiScanner):

- **WebSocket DPI Detection**: Two-stage test — holds TLS idle for 2s to detect DPI RST attacks, then checks if WebSocket upgrade works
- **SNI Rotation**: Rotates across 5 Cloudflare hostnames to reduce DPI pattern matching
- **Neighbor IP Expansion**: When an IP works, automatically probes nearby IPs (±1 to ±8) to find clusters
- **Timeout Budget Splitting**: TCP gets 1/4, TLS gets 1/2, HTTP gets 1/4 of total timeout
- **Download Speed Test**: Top candidates get a real download speed measurement
- **Multi-Phase Scanning**: Phase 1 (TCP/TLS) → Phase 2 (HTTP + DPI) → Phase 3 (Speed test for top 20)
- **Better Scoring**: Comprehensive score based on TCP, TLS, HTTP, DPI, latency, and speed

### Config Output Fixes
- Fixed HTTP 500 errors caused by malformed VMess Base64 and invalid VLESS/Trojan URIs
- All config parsing now has proper error handling with try/except
- URL-safe Base64 is properly handled
- Invalid ports and malformed configs are gracefully skipped instead of crashing
- Only configs scoring ≥ 50 are included in working_configs.txt
- Best config includes a summary with score, latency, and endpoint info

### Deploy Info Persistence
- Cloudflare API token, Account ID, Worker name, UUID, and other deploy settings are saved locally
- On next launch, form fields are auto-populated with saved values
- API token is masked in the frontend for security

### Vazirmatn Font
- The UI now uses Vazirmatn, the standard Persian web font, for proper Farsi rendering

### Open Source
- All source code is fully readable — no obfuscation or encoding
- Users can audit every line of code before running it
- Transparent and trustworthy

---

## Credits

This project builds upon ideas and methodologies from the following open-source projects. All sources are bundled internally so users never need to visit external sites:

| Project | Purpose | Repository |
|---------|---------|------------|
| **BPB Worker Panel** | VLESS-over-WebSocket worker and subscription panel | [github.com/bia-pain-bache/BPB-Worker-Panel](https://github.com/bia-pain-bache/BPB-Worker-Panel) |
| **SenPaiScanner** | Cloudflare endpoint scanning methodology | [github.com/MatinSenPai/SenPaiScanner](https://github.com/MatinSenPai/SenPaiScanner) |
| **v2ray-config-modifier** | Bulk config generation with endpoint replacement | [github.com/seramo/v2ray-config-modifier](https://github.com/seramo/v2ray-config-modifier) |

Bundled source locations:
- `integrated_sources/BPB_Worker_Panel_Bundle/` — Worker script
- `integrated_sources/SenPaiScanner_Core/` — Scanner re-exports
- `integrated_sources/Rasoul_Config_Modifier/` — Config modifier re-exports
- `integrated_sources/BPB_Wizard_Internal/` — Wizard state management

---

## Output Files

After running the application, the `output/` directory contains:

| File | Description |
|------|-------------|
| `best_active_config.txt` | The single best configuration found |
| `working_configs.txt` | All configurations that passed testing |
| `top_active_configs.txt` | Top 50 tested configurations by score |
| `base_configs.txt` | Original subscription configurations |
| `generated_configs.txt` | All generated configurations with modified endpoints |
| `scan_results.json` | Detailed test results in JSON format |
| `clean_ips.txt` | Clean IP endpoints from IP scanner |
| `ip_candidates.txt` | All scanned IP candidates |
| `ip_scan_results.json` | IP scan results in JSON format |
| `ip_scan_report_FA.txt` | Human-readable IP scan report (Farsi) |
| `report_FA.txt` | Human-readable test report (Farsi) |

---

## Troubleshooting

### Common Issues

**"No free local port found"**
- Close other applications using ports 8765-8824
- Or modify the `find_free_port()` function in `start.py` to use a different range

**"Account ID is invalid" (cfat_ or cfut_ in error)**
- You've pasted the API Token into the Account ID field
- Click "Test Token & Find Account ID" to auto-populate the correct Account ID

**"workers.dev subdomain not found"**
- Visit Cloudflare → Workers & Pages for the first time
- If prompted, choose a subdomain name
- Retry the deployment

**"Subscription returns empty or invalid"**
- Ensure the worker was deployed successfully
- Check that the UUID matches between deployment and subscription URL
- Visit the worker URL directly in a browser to verify it's running

**"No working configs found"**
- Try the IP Quality Scanner first to find good endpoints
- Switch to Clean IP mode with scanned IPs
- Increase the timeout and worker count for more thorough testing

**"Content-Type must be one of..." error during deploy**
- This version uses multipart/form-data for Module Worker uploads
- If you see this error, ensure you're running v9.0.0 or later

---

## Security Notice

- **API Tokens**: Your Cloudflare API token is only used locally to communicate with the Cloudflare API. It is never sent to any third-party server.
- **UUID**: The UUID is used only for VLESS configuration generation and is embedded in your deployed worker.
- **Local Server**: The HTTP server binds to `127.0.0.1` only — it is not accessible from other machines.
- **No Telemetry**: The application does not send any data to external servers beyond Cloudflare API calls you initiate.

---

## License

MIT License with Usage Condition

Copyright (c) 2026 mcoders

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files, to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, subject to the condition that the Software is used only with systems and accounts the user owns or is authorized to administer.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND.
