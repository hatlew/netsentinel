# NetSentinel — Network Threat Intelligence Dashboard

A fully client-side, AI-powered network security monitoring dashboard built with vanilla HTML/CSS/JS and the Claude API.

![NetSentinel Dashboard](https://img.shields.io/badge/Security-Dashboard-blue?style=flat-square) ![Claude AI](https://img.shields.io/badge/Powered%20by-Claude%20AI-purple?style=flat-square) ![Zero Dependencies](https://img.shields.io/badge/Backend-None-green?style=flat-square)

## Features

- **AI Packet Analyzer** — Paste raw tcpdump/Wireshark output and get instant AI-powered threat analysis with MITRE ATT&CK mapping, indicators of compromise, and remediation steps
- **Live Dashboard** — Real-time traffic volume charts, threat breakdown by category, and geographic threat source mapping
- **Connection Monitor** — Filterable live connection table with per-connection sparklines, protocol badges, and status classification
- **Threat Intelligence** — Active threat registry with CVE references, event counts, and investigation status
- **IOC Feed** — Indicators of compromise: malicious IPs, domains, file hashes, certificates, and user agents
- **System Health Sidebar** — Quick-glance firewall, IDS/IPS, VPN, SIEM, and honeypot status

## Tech Stack

| Layer | Choice |
|---|---|
| Frontend | Vanilla HTML/CSS/JS — zero build step |
| Charts | Chart.js 4.4 |
| AI Analysis | Anthropic Claude API (`claude-sonnet-4-6`) |
| Backend | None — fully static |

## Quick Start

1. Clone the repo
   ```bash
   git clone https://github.com/yourusername/netsentinel.git
   cd netsentinel
   ```

2. Open `index.html` in a browser — the dashboard and all simulated data work immediately with no setup

3. To use the **AI Packet Analyzer**, you need an Anthropic API key. The app calls `https://api.anthropic.com/v1/messages` directly from the browser. Add your key to your browser's request headers or set up a lightweight proxy (see below).

## API Key Setup (for the Analyzer)

The simplest approach for local use is a one-liner proxy with Node:

```bash
npx @anthropic-ai/proxy --key sk-ant-yourkey --port 8787
```

Then update the fetch URL in `index.html`:
```js
const resp = await fetch('http://localhost:8787/v1/messages', { ... });
```

For production deployment, never expose your API key client-side — route requests through a server-side function (Cloudflare Workers, Vercel Edge, AWS Lambda).

## Packet Analyzer — Sample Inputs

The analyzer ships with four built-in samples you can load with one click:

| Sample | Threat Pattern |
|---|---|
| Port Scan | Sequential SYN packets across 10+ ports from single external IP |
| C2 Beacon | Periodic fixed-interval check-ins to external host on 443 |
| SQL Injection | HTTP payloads containing `OR '1'='1'`, `UNION SELECT`, `DROP TABLE` |
| DDoS | SYN flood + UDP amplification signatures with botnet TTL variance |

Or paste your own tcpdump / Wireshark / Zeek / Suricata output.

## AI Analysis Output

For each packet capture, Claude returns:

- **Threat type & severity** (CRITICAL / HIGH / MEDIUM / LOW / CLEAN)
- **Confidence score** (0–100%)
- **MITRE ATT&CK tactics and technique IDs**
- **Indicators of Compromise** extracted from the traffic
- **Recommended actions** (block, investigate, escalate)
- **False positive likelihood**
- **Generated alerts** mapped to MITRE techniques

## Project Structure

```
netsentinel/
└── index.html      # Everything — single-file app
└── README.md
```

## Customisation

All simulated data (connections, threats, IOC feed, geo map) is generated in-browser via the JS data generators at the bottom of `index.html`. You can swap these out for real data sources by replacing the generator functions with `fetch()` calls to your own SIEM or log aggregation API.

## License

MIT
