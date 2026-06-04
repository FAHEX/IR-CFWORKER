

# 🚀 Fanmori-Edge

> **Advanced Multi-Protocol Edge Proxy & Subscription Engine Engineered for Cloudflare Workers**

**Fanmori‑Edge** is a high-performance, next-generation proxy gateway designed to deploy seamlessly on Cloudflare's global edge network. By utilizing Cloudflare Workers, it provides a robust, censorship-resistant infrastructure capable of handling advanced traffic obfuscation, multi-protocol routing, and dynamic subscription generation.

> 🌍 **Censorship Resilience Focus:** Engineered with advanced features optimized for highly restricted network environments (such as Iran). Includes native ISP-optimized clean IP pools (Irancell, MCI, Rightel), intelligent proxy chaining to bypass Deep Packet Inspection (DPI), and adaptive anti-blocking fallback layers.


1. Sub-zero ping mode (for gaming, trading and fast web browsing) 🏓
Features: Lowest CPU usage, no lag or buffering. Sites open instantly. (Download speed of large files may fluctuate slightly).

---

## ⚡ Key Highlights

```
┌────────────────────────────────────────────────────────────────────────┐
│                              CORE ENGINES                              │
├───────────────────┬───────────────────┬────────────────────────────────┤
│ 📂 PROTOCOLS      │ 🚀 TRANSPORTS     │ 🛡️ SECURITY                    │
│ • VLESS           │ • WebSocket (WS)  │ • TLS Obfuscation (uTLS)       │
│ • Trojan          │ • gRPC (GUN/Multi)│ • ECH (Encrypted Client Hello) │
│ • Shadowsocks AEAD│ • XHTTP Stream    │ • TLS Fragmentation            │
│ • SOCKS5 / HTTP(S)│                   │ • DNS-over-HTTPS (DoH) Caching │
└───────────────────┴───────────────────┴────────────────────────────────┘

```

* **Upstream Proxy Chaining:** Route outbound edge traffic through external SOCKS5/HTTP/HTTPS/TURN endpoints to mask Worker exit IPs and defeat geographical or network-level restrictions.
* **Dynamic Smart Routing:** Implements concurrent DNS resolution combined with an advanced Race Dialing system, dramatically minimizing first-packet latency.
* **Edge-Generated Subscriptions:** Native on-the-fly config provisioning for leading modern clients, including `Sing-box`, `Clash Meta`, `Surge`, `Quantumult X`, and `Loon`.
* **Administrative Dashboard:** Secure, password-protected web GUI interface directly compiled within the edge runtime for real-time config tuning, log monitoring, and resource checking.

---

## 🧩 Architectural Data Flow

```
                  ┌──────────────────────────────┐
                  │ Client (Nekobox / Sing-box)  │
                  └──────────────┬───────────────┘
                                 │
                                 │ (VLESS / Trojan / SS via WS/gRPC)
                                 ▼
                  ┌──────────────────────────────┐
                  │   Cloudflare Edge Worker     │
                  └──────────────┬───────────────┘
                                 │
         ┌───────────────────────┼───────────────────────┐
         │ [If Chaining Active]  │ [Default Path]        │ [On Direct Failure]
         ▼                       ▼                       ▼
┌──────────────────┐    ┌──────────────────┐    ┌──────────────────┐
│  Upstream Proxy  │    │  Direct Connect  │    │  Fallback Relay  │
│  (SOCKS5 / VPS)  │    │ (DoH + RaceDial) │    │   (PROXYIP Pool) │
└────────┬─────────┘    └────────┬─────────┘    └────────┬─────────┘
         │                       │                       │
         └───────────────────────┼───────────────────────┘
                                 ▼
                      ┌─────────────────────┐
                      │    Target Server    │
                      └─────────────────────┘

```

---

## 🚀 Step-by-Step Deployment Guide

### 1. Provision Edge Storage (KV Namespace)

1. Navigate to your Cloudflare Dashboard -> Workers & Pages -> KV.
2. Click Create Namespace and name it `FANMORI_KV`.
3. Copy the generated Namespace ID for the next steps.

### 2. Initialize the Edge Script

1. Navigate to Workers & Pages -> Create Application -> Create Worker.
2. Name your application (e.g., `fanmori-edge`).
3. Click Deploy -> Edit Code.
4. Replace the default boilerplate entirely with the contents of your `worker.js` file, then click Save and Deploy.

### 3. Bind Infrastructure & Configure Variables

1. Go into your Worker’s management layout -> Settings tab -> Variables.
2. Under KV Namespace Bindings, select Add Binding:
* Variable Name: `KV`
* KV Namespace: Select your previously created namespace.


3. Scroll to Environment Variables and securely assign the required parameters:

| Variable | Required | Default / Example | Operational Purpose |
| --- | --- | --- | --- |
| **`ADMIN`** | ⚠️ Critical | `YourSecurePassword123!` | Master Web GUI Access Key. Proxy features lock if left empty. |
| **`KEY`** | ⚠️ Critical | `CustomSubPath789` | Private endpoint salt for subscription routing (`/<KEY>/sub`). |
| **`UUID`** | 💡 Optional | `11111111-1111-1111-1111-111111111111` | Client credential string. Auto-derived from MD5 hash if absent. |
| **`HOST`** | 💡 Optional | `edge.yourdomain.com` | Overrides routing hostname for deterministic subscription build logic. |
| **`PROXYIP`** | 💡 Optional | `45.196.29.223:443` | Resilient infrastructure proxy fallback chain endpoints. |
| **`PRELOAD_RACE_DIAL`** | 💡 Optional | `true` | Enables background racing for ultra-low latency lookup optimizations. |
| **`OFF_LOG`** | 💡 Optional | `true` | Set to `true` to disable persistent logging to maximize Free KV write allocations. |

---

## 🔁 Proxy Chaining Options

### Option A: Admin Web Console Engine

Modify your system's global JSON layout profile variable under `反代.SOCKS5`:

```json
"SOCKS5": {
  "启用": "socks5",
  "全局": true,
  "账号": "username:password@your-vps-node.com:1080"
}

```

### Option B: Dynamic Inline Parameter Strings

Append explicit string modifiers to your operational client distribution URLs:

`https://your-worker.com/YOUR_KEY/sub?target=mixed&token=YOUR_TOKEN&socks5=user:pass@12.34.56.78:1080`

### Option C: Cryptographic Base64 Payload Paths

Generate a targeted Base64-encoded routing schema instruction pointing to your proxy endpoint, and access it directly via a structured path variant:

`https://your-worker.com/video/eyAidHlwZSI6ICJzb2NrczUiLCAidXNlcm5hbWUiOiAianVzdCIsICJwYXNzd29yZCI6ICJleGFtcGxlIiB9`

---

## 📡 Subscription Endpoints

Point your client software updates directly to these localized subscription formatting handlers:

* **Sing-Box:** `https://your-worker.com/YOUR_KEY/sub?target=singbox&token=YOUR_TOKEN`
* **Clash Meta:** `https://your-worker.com/YOUR_KEY/sub?target=clash&token=YOUR_TOKEN`
* **Surge:** `https://your-worker.com/YOUR_KEY/sub?target=surge&token=YOUR_TOKEN`
* **Quantumult X:** `https://your-worker.com/YOUR_KEY/sub?target=quanx&token=YOUR_TOKEN`
* **Raw URI List:** `https://your-worker.com/YOUR_KEY/sub?target=mixed&token=YOUR_TOKEN`

> 🔐 **Token Verification:** The verification parameter is checked via MD5(HOST + UUID). You can easily copy your pre-calculated connection tokens straight from the built-in Admin Dashboard.

---

## 📊 Constraints & Limitations

* **UDP Limitations:** Serverless edge runtimes constrain raw outbound UDP bindings. Port 53 requests are converted natively over a DoH proxy tunnel via 8.8.4.4. General online UDP multi-player gaming configurations or native standard voice calling functions are fundamentally restricted.
* **Serverless Compute Limits:** Free accounts are governed by a 10ms CPU runtime window per call instance. Extremely heavy or highly choked concurrent file-sharing tunnels may hit system compute timeouts.
* **Infrastructure Write Quotas:** Cloudflare Free tiers cap storage operations to 1,000 writes/day. To prevent hitting these limits, it is highly recommended to flag `OFF_LOG=true`.
* **Domain Interception:** Default deployment subdomains (`*.workers.dev`) face localized network disruptions across various global ISPs. Using a Custom Domain binding is highly recommended for production systems.

---

## 🛠️ Diagnostics & Troubleshooting Matrix

| System Behavior | Root Cause Vector | Corrective Engineering Action |
| --- | --- | --- |
| **`404 Not Found` Admin** | Missing runtime configurations. | Ensure the `ADMIN` security string variable is properly configured. Access `/login` directly rather than jumping straight to the interior `/admin` console. |
| **`403 Forbidden` Sub** | Invalid query signature mismatch. | Check the environmental validation hashes. Verify that `HOST` perfectly matches the incoming domain string structure exactly. |
| **gRPC Streams Hanging** | Runtime multiplexing limits. | Swap the client configuration transport mode to WebSocket (WS) or set aggressive client-side keepalive pulses. |
| **Decryption Failures** | Incompatible cipher profiles. | Force the software endpoints to use explicit modern AEAD profiles exclusively (`aes-256-gcm` or `chacha20-poly1305`). Legacy stream wrappers are rejected. |

---

## 💎 Optimized Production Guidelines

For maximum deployment stability, configure your production variables using the following optimized framework:

```ini
ADMIN = "W6k!p9Q$mZ2v_Xy7R9#bN"    # High-entropy sequence strings
UUID  = "74070776-65be-4493-8cfb-3e580e0cf476" # Formatted explicit UUIDv4 values
KEY   = "SecuredEdgeRoutingString" # Non-predictable directory paths
PRELOAD_RACE_DIAL = true            # Minimize first-packet routing overheads
OFF_LOG           = true            # Shield KV storage daily access quotas

```

---

## 📄 License

Distributed open-source under the terms of the **MIT License**. Check out the `LICENSE` document file for detailed permission scopes.

---
