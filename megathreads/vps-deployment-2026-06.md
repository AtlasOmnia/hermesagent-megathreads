# VPS & Deployment Megathread — Hermes Agent (June 2026)

> Community-maintained GitHub version of the Reddit megathread.
>
> Original Reddit thread: https://www.reddit.com/r/hermesagent/comments/1ucke01/vps_deployment_megathread_hermes_agent_june_2026/
>
> **Snapshot:** Original post preserved and normalized; comment corrections reviewed through July 16, 2026.
>
> Time-sensitive prices, quotas, versions, model availability, benchmarks, and third-party project claims remain dated snapshots unless an official source is cited.

---

**LAST UPDATED:** June 21, 2026
**Sourced from:** 9+ r/hermesagent threads (200+ comments), official Nous docs, GitHub raw docs, external deployment guides, and the Hermes Dockerfile source.

---

## TL;DR — What Should I Do?

| Decision | Community Pick | Runner-Up | Key Factor |
|----------|---------------|-----------|------------|
| Deployment type | **Bare metal on dedicated machine** | VM with snapshots (Proxmox) | Docker adds friction for most |
| VPS provider | **Hetzner** (€4-8/mo) | Netcup ($10-13/mo), Oracle free tier | Avoid Hostinger — affiliate hype |
| Remote access | **Tailscale** (mesh VPN) | Cloudflare Tunnel + auth | Zero open ports > port forwarding |
| Local alternative | **Used Dell Optiplex / Mini PC** | Raspberry Pi 5 (8GB) | $100-400 one-time vs $10-30/month |
| Docker choice | **Avoid for active dev** | Use for stable production only | Exploration = VM; stability = Docker |

---

## Part 1: The Deployment Landscape

### 🥇 Tier 1: Bare Metal / Root Install (Community Favorite)

**What it is:** Install Hermes directly on the OS — no Docker, no VM layer. `curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash`

**Why the community prefers it:**
- Hermes has full filesystem access — can install tools, manage services, fix system issues
- No Docker permission headaches, no volume mount confusion, no missing dependencies
- "Hermes needs more than a container to run optimally. It needs an entire tech stack." — u/Shady_Prospector
- "I want it to run free like a lion in Africa" — u/PurpleParrot1999
- Direct access means Hermes can fix real problems: one user described Hermes diagnosing a dual-starting systemd service with zero config — "no need to link drives, give temporary access rights, just straight: 'check why this service runs twice', and it fixed it"

**Watch for:**
- Security: Hermes has full access to your machine. Use a dedicated box or VM.
- Backups: Snapshot or back up ~/.hermes regularly. "The very second I got my Hermes config to a workable state I snapshot that MF" — u/100PercentJake
- Log rotation: Session logs and SQLite state.db fill disks fast on active use (u/Profanonyme1337)
- Systemd on Linux: `hermes gateway install --system` for auto-start on boot, clean restarts, proper logging
- Launchd on macOS: `hermes gateway install` handles this

**Best for:** Active development, tinkering, local-only setups, anyone who wants Hermes to manage the machine

---

### 🥈 Tier 2: VM with Snapshots (Sanity Saver)

**What it is:** Run Hermes in a virtual machine — Proxmox, QEMU/KVM, VirtualBox, UTM, VMware. Give it root inside the VM.

**Why:**
- Full OS access inside the VM, isolation from host
- Snapshots are the killer feature: break something? Revert in seconds
- "I prefer the hypervisor route because I can right click and hit 'snapshot' and have something to go back to" — u/100PercentJake
- Proxmox is the most-cited hypervisor across threads
- Works on everything: old laptops, N100 mini PCs, Synology NAS, Dell Optiplex

**Best for:** People who want isolation but not Docker restrictions, anyone who tinkers heavily and needs rollback

**Watch for:**
- RAM overhead: host OS + VM eats RAM. One user moved from VM to bare metal because "half my system's RAM was [used by] the host OS"
- GPU passthrough: if you're running local models, VM GPU passthrough adds complexity
- Actual guide: Install Linux (Ubuntu/Debian), install dependencies, install Hermes via one-liner, give it sudo, snapshot immediately after config is working

---

### 🥉 Tier 3: Docker (Stable Only)

**What it is:** Official `nousresearch/hermes-agent` image. s6-overlay supervised. One container can host multiple profiles.

**When Docker works:**
- Your setup is stable and you're not actively adding tools/dependencies
- You need clean separation between Hermes and host
- You're running a production gateway that shouldn't touch the host
- You know Docker well enough to troubleshoot volume mounts, permission issues, and missing tools

**When Docker fails (community consensus):**
- "The docker image for hermes is VERY limited. It doesn't have all the tools/skills hermes needs. It doesn't even have a web browser or search!" — u/halarioushandle
- "I find it tricky to keep the system stable and manage access to the host, not to mention the hassle of managing Hermes updates since I had to rebuild everything from scratch" — u/TransportationLow130
- "Dealing with the container issues is not worth it" — u/radicalscents
- "once hermes gets into container, it cannot do any system maintenance work anymore" — u/This_Maintenance_834
- Cron jobs inside Docker add complexity — Hermes runs cron internally and Docker constrains that
- Missing tools: you'll need to install skills, browsers, and system deps after container setup

**Official Docker quickstart:**
```sh
mkdir -p ~/.hermes
docker run -it --rm \
  -v ~/.hermes:/opt/data \
  nousresearch/hermes-agent setup

# Gateway mode (background):
docker run -d \
  --name hermes \
  --restart unless-stopped \
  -v ~/.hermes:/opt/data \
  -p 8642:8642 \
  nousresearch/hermes-agent gateway run

# With dashboard:
docker run -d \
  --name hermes \
  --restart unless-stopped \
  -v ~/.hermes:/opt/data \
  -p 8642:8642 \
  -p 9119:9119 \
  -e HERMES_DASHBOARD=1 \
  nousresearch/hermes-agent gateway run
```

**Resource minimums (official):**
| Resource | Minimum | Recommended |
|----------|---------|-------------|
| Memory | 1 GB | 2-4 GB (4 GB with browser tools) |
| CPU | 1 core | 2 cores |
| Disk | 500 MB | 2+ GB |

**Docker Compose example:**
```yaml
services:
  hermes:
    image: nousresearch/hermes-agent:latest
    container_name: hermes
    restart: unless-stopped
    command: gateway run
    ports:
      - "8642:8642"
      - "9119:9119"
    volumes:
      - ~/.hermes:/opt/data
    environment:
      - HERMES_DASHBOARD=1
    deploy:
      resources:
        limits:
          memory: 4G
          cpus: "2.0"
```

**⚠️ Avoid browser-based VPS consoles** (Hetzner Cloud, etc.) for the initial docker run — they corrupt special characters. Connect via SSH instead.

**Multi-profile in one container** (official, s6-supervised):
```sh
docker exec hermes hermes profile create work
docker exec hermes hermes -p work gateway start
```

**Docker pro-tip:** Mount `/var/run/docker.sock` if you want Hermes to manage other containers from inside its own container. This is an advanced pattern — u/jereminius reports it works for browser tools and container management.

---

### ⚠️ Tier 4: Hybrid / Experimental

**Host-level Hermes + Docker sub-agents** — the OP of "Docker limitations" proposed this and it was validated by multiple commenters as technically feasible:
- One Hermes installed directly on the host (root access) as "admin"
- Sub-agents in individual Docker containers for isolated tasks
- Plumbing via Ansible, Puppet, or SSH keys between them
- "Running a host-level agent that orchestrates dockerized sub-agents is doable with something like Ansible or Puppet for the plumbing" — u/Automatic-Cover-1831

**Distrobox + Quadlet** (u/Necessary_Two_9669): Containerized with better host integration than plain Docker.

**Terraform + Ansible + Podman (rootless, quadlet)** for VPS deployment (u/_zendar_).

---

## Part 2: VPS Provider Comparison

| Provider | Price/Month | Verdict | Notes |
|----------|------------|---------|-------|
| **Hetzner** | €4-8 | 🥇 Community pick | Reliable, SSH-friendly, good price/perf |
| **Netcup** | $10-13 | 🥈 Solid | Used by multiple community members |
| **Oracle Cloud** | Free tier | 🥈 Best free option | Generous free tier, block storage, Tailscale-friendly. Setup is "corporate tech" (not user-friendly). Quality can degrade after initial allocation. Limited availability by region. |
| **AWS EC2** | Varies ($200 free credit) | 🥉 Temporary | Free credits work but burns fast. ~$1.50/day on modest instance. |
| **Hostinger** | Varies | ⚠️ Avoid | Called "biggest culprit" in affiliate marketing by multiple users. "These people are getting paid big bucks" — u/Creative_Diver3492 |
| **Hetzner Storage Share** | $4/mo | 🥉 | Good companion storage, not for compute |

**Real community budget (from u/Background-Remote765):** Netcup VPS ($10-13/mo) + Hetzner Storage ($4/mo) + DeepSeek API ($5-10/mo) = **~$30-35/month total** — replacing Spotify, Google ecosystem, and AI subscriptions.

---

## Part 3: Networking & Remote Access

### 🥇 Tailscale (mesh VPN) — Community Pick

"Use Tailscale, runs cleanly" — u/xyesca

- Zero open ports. Hermes dashboard accessible only to devices on your Tailscale network.
- Works with VPS, home server, and mobile.
- Free for personal use (up to 100 devices).
- Combined with Cloudflare Tunnel for public-facing services if needed.

### 🥈 Cloudflare Tunnel

- Access Hermes Dashboard from anywhere without opening ports.
- **Security note:** Keep "mission control" (third-party admin UIs) on Tailscale only, not on public Cloudflare tunnels — "mission control exposed on a public tunnel is a wider attack surface than you need" — u/Profanonyme1337
- Combine with Cloudflare Zero Trust for authentication layer.

### 🥉 SSH Tunnels (Manual)

The OP of the SSH tunnel thread found a working pattern:
1. SSH config forwards local ports to remote VPS
2. Dashboard accessible at `http://127.0.0.1:9119` locally
3. Hermes Desktop connects to that forwarded port

**Critical fix they discovered:** Enable the `dashboard_auth/basic` plugin. Without it, the dashboard silently fails to start when running as a systemd background process — and the Desktop app can't authenticate.

### Other options mentioned: NetBird, Pangolin (self-hosted VPN)

---

## Part 4: Security Hardening

### Community-Vetted Practices

**Minimal VPS setup** (from u/orthogonal-ghost on Hetzner):
1. Create a non-root `hermes` user with sudo (but don't give it the sudo password — Hermes prompts when it needs elevation)
2. Disable root login and password authentication in SSH config
3. Restrict firewall to approved IPs only (or Tailscale-only access)
4. Keep Hermes on its own VPS — don't share with other services

**Dashboard security** (official docs, hardened June 2026 after MCP-config persistence campaign):
- The dashboard auth gate is now **mandatory** on non-loopback binds
- Easiest path: `HERMES_DASHBOARD_BASIC_AUTH_USERNAME` + `HERMES_DASHBOARD_BASIC_AUTH_PASSWORD`
- For public exposure: OAuth via Nous Portal or self-hosted OIDC
- **Never** expose an unauthenticated dashboard to the internet

**Credential protection:**
- Use AgentVault (mentioned by u/AnticitizenPrime) to prevent credential leaks
- Nightly backups to external drive
- Hermes built-in safeguards prevent leaking env vars/credentials (u/Howard_banister)

**The "full send" approach** (u/p_viljaka):
"i gave it its own linux VPS and basically 'full send', it has root / sudo — yolo, i told its his own box and take care of it and dont fuck around, and no more box'es if it screws up. It seems happy."

This works because: Hermes on its own VPS with no shared services can't hurt anything beyond itself. Combined with backups, this is lower-risk than it sounds.

---

## Part 5: Local Hardware Alternatives

Skip the VPS entirely. The community overwhelmingly prefers local hardware:

| Hardware | Cost | RAM | Notes |
|----------|------|-----|-------|
| Used Dell Optiplex | ~$100 | 32GB DDR4 | Homelab gold. Proxmox-ready. |
| Raspberry Pi 5 | ~$80 | 8GB + NVMe | "No issues so far" for gateway-only |
| Used Mac Mini | ~$200-400 | 16GB+ | "No fan, low consumption, works perfect" |
| Intel N100 Mini PC | ~$150 | 16GB | "Smallest VM possible was enough" |
| Old laptop (any) | Free-$100 | Varies | Reformat with Linux, install Hermes directly |
| Orange Pi w/ NPU | ~$100 | Varies | Self-hosted with NPU acceleration |
| HP ProDesk (salvaged) | Free | Varies | "Found being thrown away on the side of the road" |

**Why local wins:**
- No monthly cost after hardware purchase
- No latency between you and your agent
- Full control — no provider terms of service, no data egress fees
- "No tailscale, no vps, no costs whatsoever" — u/[deleted] running Qwen 3.6 locally behind router firewall with Discord gateway

**The used Mac Mini pattern** (u/RPendragon_ & Jonathan_Rivera): Ideal always-on Hermes host — silent, low power, runs macOS natively for Apple ecosystem integrations (Reminders, Notes, iMessage, AppleScript, CUA driver).

---

## Part 6: Browser Automation — The #1 VPS Frustration

This deserves its own section because it's the single biggest complaint across all VPS threads.

**The problem:** Headless browsers on VPS datacenter IPs get flagged by WAF/anti-bot systems constantly — Cloudflare, DataDome, Akamai all block datacenter IP ranges.

**Community solutions (tiered):**

1. **Avoid browser automation when possible.** "Agents speak and read text way better than puppeting a browser and screenshotting" — u/Starrwulfe (+25). Use APIs and CLIs instead: Exa for search, Himalaya for email, CLI tools for everything else.

2. **Anti-detection browsers:**
   - Camofox (`github.com/jo-inc/camofox-browser`) — built for this, free
   - Camoufox — C-level anti-detection, free, more aggressive fingerprinting
   - Note: these are different browsers. Camofox ≠ Camoufox.

3. **Residential proxies** — Add a residential proxy layer. Costs money but bypasses datacenter IP blocks.

4. **CAPTCHA services:** unblockingapi.com, Zyte, Scrapingbee — for sites that throw CAPTCHAs.

5. **Browserbase / Browserless free trials** — Cloud browser services with better IP reputation. Free tiers exist.

6. **Firecrawl** — worked for OP where Camoufox failed on real estate sites.

**The real lesson:** If you need heavy browser automation, a VPS is the wrong platform. Run it locally or use API-first alternatives.

---

## Part 7: Log Management

**You will run out of disk.** Multiple users reported this.

- Session logs live in `~/.hermes/sessions/`
- SQLite state database at `~/.hermes/state.db`
- Gateway logs at `~/.hermes/logs/gateways/<profile>/current`

**For Docker:** Gateway logs are tee'd to both `docker logs` AND rotated files on the bind-mounted volume (10 archives × 1 MB each per profile). The boot reconciler writes to `~/.hermes/logs/container-boot.log`.

**Action items:**
- Set up logrotate for `~/.hermes/sessions/` and `~/.hermes/logs/`
- Monitor disk usage: "session logs and the SQLite state.db will fill a 20GB disk faster than you'd expect on active use" — u/Profanonyme1337
- Docker: `docker logs` only covers current container lifetime; rotated files on volume persist

---

## Part 8: FAQ

**Q: Should I use Docker for Hermes?**
A: Not if you're still exploring or adding tools. Docker is good for stable production setups where the toolset is locked in. For active development, use bare metal or a VM with snapshots.

**Q: What's the cheapest way to run Hermes 24/7?**
A: A used Dell Optiplex (~$100) or Raspberry Pi 5 (~$80). One-time cost, zero monthly fees. If you need cloud, Oracle free tier or Hetzner at €4/mo.

**Q: How do I access my VPS Hermes securely?**
A: Tailscale. No open ports. Access dashboard, API, and SSH from anywhere on your mesh network. Free for personal use.

**Q: Why does my browser automation fail on VPS?**
A: Datacenter IPs are flagged by anti-bot systems. Use Camofox browser, residential proxies, or (better) API/CLI alternatives.

**Q: Can Hermes run multiple profiles in one Docker container?**
A: Yes — the official image uses s6-overlay to supervise per-profile gateways. `docker exec hermes hermes profile create <name>` is all you need. No second container.

**Q: Should I expose my Hermes dashboard to the internet?**
A: If you must, enable authentication. The dashboard **refuses to start** on a public bind without an auth provider as of June 2026. Use basic auth (`HERMES_DASHBOARD_BASIC_AUTH_USERNAME` + `_PASSWORD`) or OAuth. Better: keep it on Tailscale.

**Q: How much does running Hermes on a VPS actually cost?**
A: Community reports: $30-35/month all-in (VPS + storage + API tokens) for an active setup. Local hardware breaks even in 3-6 months.

**Q: What's the "Hermes Desktop to VPS" connection path?**
A: SSH tunnel to forward port 9119 → enable `dashboard_auth/basic` plugin → point Hermes Desktop to `http://127.0.0.1:9119`. Without the auth plugin, the dashboard fails silently.

**Q: Can I run Hermes on a Raspberry Pi?**
A: Yes — multiple users confirm Pi 5 (8GB) + NVMe works for gateway mode. Not for running local LLMs, but fine as a 24/7 gateway to cloud APIs.

**Q: What about Proxmox?**
A: Heavily endorsed. Run Hermes in an LXC or VM, snapshot the working config, roll back instantly if something breaks. "Snapshot that MF" — u/100PercentJake

---

## Part 9: Knowledge Table — Every Tool & Service Mentioned

| Name | Type | Cost | Best For | Watch For |
|------|------|------|----------|-----------|
| **Hetzner** | VPS | €4-8/mo | Production Hermes hosting | Browser console corrupts paste |
| **Oracle Cloud** | VPS | Free tier | Budget 24/7 hosting | Limited availability, degrades |
| **Netcup** | VPS | $10-13/mo | Budget VPS | Less known, fewer guides |
| **AWS EC2** | Cloud | $200 free credit | Trial/temporary | Credits burn ~$1.50/day |
| **Tailscale** | Mesh VPN | Free (personal) | Secure remote access | Requires client on each device |
| **Cloudflare Tunnel** | Tunnel | Free | Public access without ports | Don't expose admin UIs on it |
| **Proxmox** | Hypervisor | Free | VM snapshots, homelab | RAM overhead for host OS |
| **Camofox** | Browser | Free | Anti-detection browsing | Different from Camoufox |
| **Camoufox** | Browser | Free | C-level anti-detection | Heavier fingerprinting |
| **Firecrawl** | Web scraping | Paid | Sites that block headless | Not free at scale |
| **Browserbase** | Cloud browser | Free trial | CAPTCHA-heavy sites | Trial limited |
| **Exa** | Search API | Paid | Search without browser | API cost at scale |
| **Himalaya** | Email CLI | Free (built-in) | IMAP/SMTP in Hermes | Proton needs paid tier for IMAP |
| **Composio** | API integration | ? | Connecting services | Newer tool |
| **Dokploy** | Docker manager | Free | Managing Docker Hermes | Adds another layer |
| **AgentVault** | Credential protection | ? | Preventing API key leaks | Setup overhead |
| **Zyte / Scrapingbee** | CAPTCHA solving | Paid | Unblocking headless browsers | Ongoing cost |
| **unblockingapi.com** | CAPTCHA solving | Paid | Alternative to Zyte | Less established |
| **Meragpt.com** | Managed Hermes | Paid | Zero-setup Hermes hosting | Vendor lock-in |
| **Distrobox + Quadlet** | Container | Free | Docker-like with host access | Podman knowledge needed |
| **TinyAgentOS** | Bare-metal agent OS | Free | Streamlined Hermes on metal | github.com/jaylfc/tinyagentos |
| **Fox-in-the-Box** | Docker Hermes | Free | Hermes-specific Docker build | github.com/fox-in-the-box-ai |

---

## Part 10: The Bottom Line

The community has spoken clearly: **Docker is the wrong default for Hermes.** It's a production optimization, not a starting point. Start on bare metal or a VM, give Hermes the freedom it needs, snapshot often, and only containerize once your toolset is stable and you know exactly what you're doing.

If you're paying for a VPS and burning API tokens while fighting browser automation — you're doing it on hard mode. A $100 used PC running Tailscale will get you further, faster, with less frustration.

---

*This megathread synthesized from 9+ r/hermesagent threads (200+ comments), official Nous Research Docker documentation, the Hermes Dockerfile source, and multiple external deployment guides. Corrections and additions welcome in the comments.*

---

## Comment-Sourced Updates

- **Canonical VPS source:** this June 21 guide supersedes the earlier June 3 and April deployment summaries. Unique older material should be folded here only after verification.

- **Docker sequencing:** commenters correctly distinguish “good production/stability boundary” from “easiest place to experiment.” Start with the simplest topology that meets the isolation requirement.

- **Community deployment projects:** taOS and external setup curricula were suggested; they remain dated external leads pending maintenance and security review.

## Maintaining this guide

Open an issue or pull request with the official source, date checked, Hermes/backend version, and enough reproduction detail to evaluate the change. No referral links, affiliate links, or unsupported promotional claims.
