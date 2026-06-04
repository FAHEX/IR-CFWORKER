# 📘 Fanmori-Edge – Features, Limitations, Setup Guide & Variables (In-Depth for Iranian Users)

This document provides a **complete technical and practical review** of the **Fanmori-Edge** Cloudflare Worker project.  
Its primary goal is **bypassing censorship** and providing **free internet access**, with special attention to the needs of Iranian users.

---

## ✅ Key Advantages (Why Fanmori-Edge is Great for Iran)

| Advantage | Explanation |
|-----------|-------------|
| **Proxy Chaining (Looping)** | You can route traffic through an upstream proxy (e.g., a SOCKS5 on a VPS) before reaching the final destination. This makes you harder to track and allows you to use clean private IPs. |
| **Clean IP Pools for Iranian ISPs** | Built‑Ps** | Builtin support‑in support for IPs for IPs optim optimised for Irancellised for Irancell, Ham, Hamrahrah Aval Aval, M, MCI, and otherCI, and other local local operators operators. These. These IPs IPs offer low ping and offer low ping and stable download speeds stable download speeds. |
| **. |
| **TrafficTraffic O Obfuscation insidebfuscation inside TLS** | TLS** | All All traffic is wrapped traffic is wrapped inside WebSocket/g inside WebSocket/gRPC andRPC and then encrypted then encrypted with real TLS (using with real TLS (using Chrome/F Chrome/Firefox fingerprints). Very hard forirefox fingerprints). Very hard for DPI systems DPI systems to detect. |
| **Multi to detect. |
| **Multi‑Protocol Support**‑Protocol Support** | Works | Works with VLESS, with VLESS, Trojan, Shadowsocks (AE Trojan, Shadowsocks (AEAD). CompatAD). Compatible with popular clients: Nekible with popular clients: Nekobox, v2obox, v2rayNG, ClrayNG, Clash, Sing‑box, Surge, etc. |
| **Automaticash, Sing‑box, Surge, etc. |
| **Automatic Subscription Generation** | Set Subscription Generation** | Set up once up once, then just give your clients, then just give your clients a single a single subscription link. Any subscription link. Any changes (new changes (new clean clean IPs, IPs, path updates) are applied automatically. |
| path updates) are applied automatically. |
| **Web‑Based **Web‑Based Admin Panel** | No need for Admin Panel** | No need for SSH or command SSH or command line. Change line. Change settings settings, view, view logs, test logs, test upstream proxies, and upstream proxies, and check Cloudflare usage check Cloudflare usage directly from your browser directly from your browser. |
| **. |
| **BuiltBuilt‑in DNS‑in DNS‑over‑over‑HTTPS with‑HTTPS with Caching & Caching & Race Race Dial Dial** | Enc** | Encrypted DNS queries,rypted DNS queries, cached results, and concurrent cached results, and concurrent connection connection attempts to the attempts to the fastest resolved fastest resolved IP – dramatically improves direct connection speed. |
| **No External Server IP – dramatically improves direct connection speed. |
| **No External Server Required Required ( (Only CloudflareOnly Cloudflare Workers)** | Free plan gives 100k requests/day – Workers)** | Free plan gives 100k requests/day – enough for most personal enough for most personal use. |
| use. |
| **Hard **Hard to Detect** to Detect** | Features like ` | Features like `uTLS` (imitating real browsers), `uTLS` (imitating real browsers), `ECH` (EncECH` (Encrypted Client Hellorypted Client Hello),), and `TLS and `TLS Fragment` help bypass Fragment` help bypass many censorship many censorship systems. |

 systems. |

---

##---

## ❌ Limitations ❌ Limitations & Draw & Drawbacks

| Limbacks

| Limitation | Explanationitation | Explanation |
|------------|------------- |
|------------|-------------|
| **U|
| **UDP onlyDP only for port for port 53 (DNS 53 (DNS)** | Online)** | Online gaming, VoIP gaming, VoIP,, and any non and any non‑DNS UDP traffic‑DNS UDP traffic are are rejected. |
| rejected. |
| **Free plan **Free plan CPU limit CPU limit** | Each** | Each request has only request has only 10ms of 10ms of CPU time. Heavy downloads may CPU time. Heavy downloads may be interrupted. be interrupted. |
| **KV |
| **KV storage storage limits ( limits (free planfree plan)** |)** | Only Only 1k 1k reads and reads and 1k 1k writes per day. Not writes per day. Not suitable suitable for heavy for heavy logging. |
| logging. |
| **Initial setup complexity **Initial setup complexity** | Non** | Non‑technical users may‑technical users may find KV find KV binding binding and environment and environment variables confusing variables confusing. |
| **. |
| **Custom domain is highlyCustom domain is highly recommended** | Using the recommended** | Using default `*. the default `workers.dev` domain*.workers.dev` might domain might be blocked by some ISPs. A be blocked by some ISPs. A personal domain (even free like `eu personal domain (even free like `eu.org`).org`) works works much much better. better. |
 |
| **TURN| **TURN / SSTP protocols / SSTP protocols are unstable** | are unstable** | They require extra They require extra round trips round trips and often and often time time out on Cloud out on Cloudflareflare Workers. |

 Workers. |

---

## 🛠---

## 🛠️ Step‑by️ Step‑by‑Step Installation Guide‑Step Installation Guide (for Iranians)

### Step (for Iranians)

### Step 1: Create 1: Create a Cloudflare Account a Cloudflare Account & Enable Workers
- Sign & Enable Workers
- Sign up at up at [cloudflare.com [cloudflare.com](https://cloudflare.com) if](https://cloudflare.com) if you don’t you don’t have an account have an account.
- Go.
- Go to ** to **Workers & PagesWorkers & Pages** and** and create a new create a new Worker (e.g Worker (e.g., `fanm., `fanmori-edge`ori-edge`).

### Step 2).

### Step 2: Create a KV: Create a KV Namespace (to Namespace (to store config store configs)
- Froms)
- From the left menu the left menu → **Work → **Workers & Pages**ers & Pages** → **KV** → **KV**.
- Create a.
- Create a new namespace (e new namespace (e.g., `.g., `FANMORFANMORII_KV`).
- Copy_KV`).
- Copy its ** its **ID** – youID** – you’ll need it’ll need it later.

### Step later.

### Step 3: 3: Insert Insert the Worker Code
- Open the Worker Code
- Open your newly created your newly created Worker.
- Worker.
- Click Click **Quick Edit**.
- Delete **Quick Edit**.
- Delete all existing all existing code, code, then paste then paste the entire `fan the entire `fanmorimori-edge`-edge` script.
- Click script.
- Click **Save and Deploy**.

### Step 4 **Save and Deploy**.

### Step: Bind KV to the Worker 4: Bind KV to
- Go to the Worker
- Go your to your Worker’s **Settings** → ** Worker’s **SettingsVariables** → **** → **Variables** → **KV Namespace BindKV Namespace Bindings**.
- Click **Add binding**:
  -ings**.
- Click **Add binding**:
  - Variable name: ` Variable name: `KV`
  -KV`
  - KV Names KV Namespace: choose the onepace: choose the one you created.
- you created.
- Save.

### Step  Save.

### Step 5:5: Set Set Environment Variables (Critical)
Still under **Settings** → **Variables** Environment Variables (Critical)
Still under **Settings** → **Variables →** **Environment Variables**, add → **Environment Variables:

| Variable | Recommended**, add:

| Variable | Recommended Value | Explanation Value | Explanation |
 |
|----------|-------------------||----------|-------------------|-------------|
| `ADMIN-------------|
| `ADMIN` | `a-` | `a-strong-password-strong-password-88+chars` |+chars` | Password for the admin panel Password for the admin panel.. |
| `KEY |
| `KEY` | `a-random-string-like` | `a-random-string-like " "s3cr3s3cr3tPatht"` | Access path forPath"` | Access path for subscription and proxy. |
| ` subscription and proxy. |
| `UUID` | A standard UUID eUUID` | A standard UUID e.g.,.g., `11111111-1111- `11111111-11111111-111-1111-1111-1111111-111111111111` | User ID for VL111111` | User ID for VLESS/TrojanESS/Trojan. |
| `. |
| `PROXYIP` | (OptionalPROXYIP` | (Optional) e.g.,) e.g., `ir-pro `ir-proxy.examplexy.example.com:443, 1.2.3..com:4434:8080, 1.2.3.` | Fallback proxy IPs4:8080` | Fallback proxy IPs/domains/domains. |
| `. |
| `HOST`HOST` | The | The domain your Worker runs domain your Worker runs on ( on (e.g., `myworkere.g., `myworker.yourdomain.com.yourdomain.com`) | Required for correct`) | Required for correct subscription subscription links. |
| links. |
| `DEBUG` | `false` ( `DEBUG` | `false` (default)default) or `true` | Keep `false` to or `true` | Keep `false` to reduce logging. reduce logging. |
| `PRELOAD_RACE |
| `PRELOAD_RACE_DIAL` |_DIAL` | `true` | Enables concurrent `true` | Enables concurrent DNS pre‑resolve DNS pre‑resolve for for faster direct faster direct connections. |

> **Important**: connections. |

> **Important**: If you don If you don’t set `’t set `ADMIN`, theADMIN`, the proxy functionality will be proxy functionality will be **disabled**.

### **disabled**.

### Step 6: Step 6: Finalise Finalise and and Test
- Your Test
- Your Worker is now live Worker is now live at: `https at: `https://<worker-name://<worker-name>.<sub>.<subdomain>.workers.devdomain>.workers.dev`
- Admin`
- Admin panel: `https panel: `https://.../login` –://.../login` – use use your your `ADMIN` `ADMIN` password.
- After login, you can password.
- After login, you can edit `config.json` to edit `config.json` to fine‑tune proxy settings fine‑tune proxy settings, subscriptions,, subscriptions, clean IP pools, etc.

 clean IP pools, etc.

---

## 🌐 Detailed---

## 🌐 Detailed Explanation Explanation of Environment Variables of Environment Variables

| Name

| Name | Type | Type | Required | Effect | Required | Effect |
|------| |
|------|------|----------|------|----------|--------|
| `--------|
| `ADMIN` |ADMIN` | string | ✅ Yes string | ✅ Yes | Admin | Admin panel password panel password. Without. Without it, the Worker it, the Worker only shows error only shows error pages. pages. |
| ` |
| `UUID` |UUID` | UUID UUID string | ✅ Yes string | ✅ Yes | User | User ID for VLESS ID for VLESS/Trojan//Trojan/SS. If notSS. If not set, it set, it is derived is derived from ` from `MD5(ADMD5(ADMIN+KEYMIN+KEY))`. |
| ``. |
| `KEY` |KEY` | string string | ✅ Yes | | ✅ Yes | Access path for subscription Access path for subscription and WebSocket. Example: if and WebSocket. Example: `KEY=myapp`, the if `KEY=my subscription endpointapp`, the subscription endpoint becomes becomes `/myapp`. `/myapp`. |
| `PRO |
| `PROXYXYIP` |IP` | comma-separ comma-separated list |ated list | ❌ No | Fall ❌ No | Fallback proxiesback proxies. When direct. When direct connection fails, connection fails, the Worker uses the Worker uses these. Can these. Can be IP be IP::port or domainport or domain names names that resolve that resolve to IP to IPss via via TXT records. TXT records. |
| `H |
| `HOST` | string | ❌ NoOST` | string | ❌ No | The | The domain your Worker responds domain your Worker responds on on. Needed. Needed for correct for correct subscription generation and subscription generation and SN SNI.I. |
| `PATH |
| `PATH` | string |` | string | ❌ No | ❌ No | Base path for Web Base path for WebSocket/gRPCSocket/gRPC. Default is. Default is `/`. |
| `/`. |
| `BEST_SU `BEST_SUB` | `B` | `true`/true`/`false` | ❌ No | When `true`, generates`false` | ❌ No | When `true`, generates an an optim optimised subscription with cleanised subscription with clean IPs specifically IPs specifically for Iranian for Iranian IS ISPs. |
|Ps. |
| `PRELOAD_R `PRELOAD_RACE_DIAL`ACE_DIAL` | `true` | `true`/`false` | ❌/`false` | ❌ No No | When | When `true `true`, the`, the Worker performs Do Worker performs DoH lookH lookups and connects to multiple resolvedups and connects to multiple resolved IPs simultaneously, IPs simultaneously, picking the fastest picking the fastest.. Bo Boosts speed.osts speed. |
| `DEBUG` |
| `DEBUG` | `true` | `true`/`false`/`false` | ❌ No | ❌ No | En | Enables detailedables detailed logging logging.. Keep ` Keep `false` for normalfalse` for normal use use. |

---

##. |

---

## 🔁 What is 🔁 What is Proxy Chaining ( Proxy Chaining (Looping)Looping) and and Why Does It Matter Why Does It Matter for Iran?

**Proxy chaining** means your for Iran?

**Proxy chaining** means your traffic passes through multiple traffic passes through multiple servers before reaching its servers before reaching its destination.  

Example destination.  

Example:  
`Your client:  
`Your client → Cloud → Cloudflare Worker → SOCflare Worker → SOCKS5 proxyKS5 proxy on a German VPS on a German VPS → Final target → Final target (e.g., (e.g., Google)`

### Google)`

### Why it’s useful Why it’s useful for for Iranian users:
- Iranian users:
- **Increased reliability**: **Increased reliability**: If Cloudflare Workers If Cloudflare Workers get blocked get blocked, your VPS might, your VPS might still work still work.
- **Better.
- **Better anonymity**: The anonymity**: The final target final target only sees the only sees the VPS IP VPS IP, not the Worker, not the Worker IP IP.
- **.
- **Use yourUse your own clean own clean IP**: IP**: You can rent You can rent a cheap a cheap VPS with a clean IP and VPS with a let the Worker act as clean IP and let a front‑end the Worker act as a front.

### How to enable chaining‑end.

### How to enable chaining in in Fanmori-Edge:
- Via Admin Panel → `config.json` → Fanmori-Edge:
- Via Admin Panel → `config.json` → fill in fill in the `反 the `反代.SOCKS5` section.
- Or directly in代.SOCKS5` section.
- Or directly in the subscription link: add `?socks5= the subscription link: add `?socks5=user:pass@hostuser:pass@host:port` parameter:port` parameter.
- Or encode.
- Or encode an an upstream proxy JSON upstream proxy JSON as as base64 and append base64 and append to to the the WebSocket path: WebSocket path: `/video `/video/<base64_json>`.

/<base64_json>`---

## 📞 Support &.

---

## 📞 Support & Community

- Report Community

- Report issues issues on [GitHub Issues](https://github.com/fanmori/fanmori-edge/issues on [GitHub Issues](https://github.com/fanmori/fanmori-edge/issues)
- Telegram group:)
- Telegram group: [@Fanm [@Fanmori_Edge](ori_Edge](https://t.me/Fanmorihttps://t.me/Fanmori_Edge) *(_Edge) *(example linkexample link – replace – replace with actual)*

 with actual)*

---

## 💎 Final Summary

**Fan---

## 💎 Final Summary

**Fanmori-Edge** is amori-Edge** is a powerful powerful, lightweight, lightweight proxy proxy tool designed for tough censorship environments like Iran tool designed for tough censorship environments like Iran.  
With.  
With correct correct setup (strong setup (strong ` `ADMIN` passwordADMIN` password, a personal domain, and, a personal domain, and ` `PREPRELOAD_RACE_DIAL=true`LOAD_RACE_DIAL=true), you can enjoy`), you can enjoy stable, stable, fast fast, and unc, and uncensored internet access for yearsensored internet access for years.

**Stay.

**Stay safe safe and free and free!**  
**!**  
**#FreeInternet ##FreeInternet #CloudflareWorkers #FanmCloudflareWorkers #FanmoriEdge**
```oriEdge**
