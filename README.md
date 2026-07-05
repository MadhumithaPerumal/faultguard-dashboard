# ⚡ FaultGuard — Real-Time System Fault Monitor

A lightweight, browser-based dashboard that **simulates and detects system faults** (CPU, Memory, Disk, Services) in real time — generates **step-by-step fix instructions** automatically, and can now call **Google's Gemini API** to write a custom fix plan for any fault you describe in plain English.

Built with pure **HTML + CSS + JavaScript**. No frameworks, no backend required — the Gemini feature is optional and calls Google's API directly from your browser.

---

## 🚀 Live Demo

> Deploy to GitHub Pages → Settings → Pages → Source: `main` branch → `/root`

Your dashboard will be live at:
```
https://<your-username>.github.io/faultguard/
```

---

## 📸 Features

| Feature | Description |
|--------|-------------|
| 📊 **Live Metric Cards** | CPU, Memory, and Disk usage with animated progress bars |
| 🔴 **Fault Detection** | Auto-detects WARN / CRIT thresholds and logs every fault |
| 🧰 **Auto-Fix Playbooks** | Click any fault log entry to see step-by-step fix instructions |
| 🤖 **AI Fault Diagnosis (Gemini)** | Describe any fault in your own words and get a custom, AI-generated step-by-step fix plan |
| 💥 **Simulation Buttons** | Trigger CPU spikes, memory leaks, and service failures manually |
| 🛠 **Service Monitor** | 6 simulated services (Nginx, MySQL, Redis, Node.js, SSH, Cron) |
| ✅ **Heal All** | One-click reset to restore all systems to healthy state |
| 🕐 **Live Clock** | Real-time clock displayed in the header |

---

## 🤖 AI Fault Diagnosis (New)

Below the fault log there's an **AI Fault Diagnosis** panel. Type a description of any fault you're seeing — real or hypothetical — and Gemini will return a numbered, step-by-step troubleshooting plan (with the terminal commands you'd run on a real Linux box).

**How it works**
1. Get a free Gemini API key from [Google AI Studio](https://aistudio.google.com/apikey).
2. Paste it into the **"Paste your Gemini API key"** field and click **Save Key**.
3. Describe the fault (or click **"Use latest logged fault"** to pull in the most recent WARN/CRIT entry from the simulation).
4. Click **Generate Fix Plan**.

**How the key is handled**
- The key is stored **only** in your own browser's `localStorage` — it is never written into `index.html`, never committed to the repo, and never sent to any server other than Google's Gemini endpoint.
- Because this project has no backend, the key does live in your browser while you use the page. Don't use this pattern for a key you care about keeping fully private on a public multi-user site — for personal/local/learning use it's fine, but for a production deployment you'd want a small server-side proxy so the key never touches the client at all.
- Anthropic/Claude API keys and Gemini API keys are different things — make sure you're pasting a **Gemini** key (from Google AI Studio), and never paste any API key into a chat, README, or public repo.

**Important:** this feature only *drafts suggestions* — it never executes anything on a real system. Every command it prints is meant to be reviewed and run manually. If you want the dashboard to actually read real CPU/memory/disk/service data instead of simulated numbers, that requires a small local backend (see "Going Further" below), since browsers cannot query real system stats directly.

---

## 📁 Project Structure

```
faultguard/
├── index.html    ← Everything is here (HTML + CSS + JS, incl. Gemini integration)
└── README.md     ← This file
```

---

## 🛠️ How to Deploy on GitHub Pages

### Step 1 — Create a GitHub Repository

1. Go to [github.com](https://github.com) and click **New Repository**
2. Name it `faultguard` (or anything you like)
3. Set it to **Public**
4. Click **Create Repository**

### Step 2 — Upload the Files

**Option A — Using GitHub Web UI (easiest):**
1. On your repo page click **"uploading an existing file"**
2. Drag and drop `index.html` and `README.md`
3. Click **Commit changes**

**Option B — Using Git CLI:**
```bash
git init
git add index.html README.md
git commit -m "Initial commit: FaultGuard dashboard"
git branch -M main
git remote add origin https://github.com/<your-username>/faultguard.git
git push -u origin main
```

### Step 3 — Enable GitHub Pages

1. In your repo, go to **Settings → Pages**
2. Under **Source**, select `Deploy from a branch`
3. Choose branch: `main`, folder: `/ (root)`
4. Click **Save**
5. Wait ~60 seconds, then visit your live URL!

> ⚠️ Never commit an API key into `index.html` or any file in the repo. The Gemini key field is meant to be filled in by each visitor in their own browser, not hardcoded before you push.

---

## 🎮 How to Use FaultGuard

### Simulation Buttons (top bar)
| Button | What it does |
|--------|-------------|
| ⚠️ Simulate CPU Spike | Pushes CPU to 90%+ for ~12 seconds |
| 🧠 Simulate Memory Leak | Gradually increases memory until critical |
| 💥 Simulate Service Fail | Randomly kills one of the 6 services |
| ✅ Heal All Systems | Resets everything back to normal |
| 🗑 Clear Log | Clears the fault log entries |

### Fault Log
- Every fault is logged with a **timestamp** and **severity level** (WARN, CRIT, OK, INFO)
- Click any **WARN** or **CRIT** log entry to open the **auto-fix modal** (built-in playbook)
- Or scroll to the **AI Fault Diagnosis** panel to get a Gemini-generated plan for any fault, including ones the simulator didn't trigger

### Services Panel
- Each service card shows **Running** or **FAILED** status
- Click **Kill** to simulate a service failure
- Click **Start** to restore a stopped service

---

## 🎨 Severity Levels

| Level | Color | Meaning |
|-------|-------|---------|
| `OK` | 🟢 Green | System is healthy |
| `WARN` | 🟡 Yellow | Approaching threshold — take note |
| `CRIT` | 🔴 Red | Threshold exceeded — action required |
| `INFO` | 🔵 Blue | Informational message |

---

## 📚 Thresholds Reference

| Metric | WARN | CRIT |
|--------|------|------|
| CPU Usage | > 70% | > 90% |
| Memory Usage | > 65% | > 85% |
| Disk Usage | > 75% | > 92% |

---

## 🧑‍💻 Tech Stack

- **HTML5** — Structure and layout
- **CSS3** — Custom properties, animations, grid, flexbox
- **Vanilla JavaScript** — All logic, simulation, DOM updates, and the Gemini API call
- **Gemini API** (`gemini-2.5-flash`, `generateContent` endpoint) — powers the AI Fault Diagnosis panel
- **Google Fonts** — Share Tech Mono + Exo 2 (loaded via CDN)

> ⚠️ Metric and service data is **simulated** in the browser — this dashboard does not read real system stats. Only the AI Fault Diagnosis panel talks to a real external service (Gemini), and only with a key you provide.

---

## 🔭 Going Further — Real System Monitoring

This project is intentionally a simulation for learning purposes. If you want FaultGuard to react to a *real* machine's CPU/memory/disk/services instead of simulated numbers, you'd need to add a small backend, because browsers cannot read OS-level stats directly:

- A lightweight local agent (e.g. Node.js with the `systeminformation` package, or a script reading `/proc`, `df`, `systemctl`) that exposes real metrics over a local HTTP endpoint.
- The dashboard fetches from that local endpoint instead of the built-in simulation loop.
- Keep any Gemini/Claude API key on that backend, never in the browser, once real infrastructure is involved.
- If you ever want the AI to *apply* a fix automatically rather than just suggest one, gate it behind an explicit user confirmation and an allow-list of safe commands — never let an LLM run arbitrary shell commands unsupervised on a real system.

---

## 💡 Learning Objectives

This project helps students understand:
- Real-time UI updates using `setInterval`
- DOM manipulation and dynamic rendering
- CSS custom properties (variables) for theming
- CSS animations (`@keyframes`, transitions)
- Event-driven programming (click handlers, state changes)
- System monitoring concepts (CPU, memory, disk, services)
- DevOps fault-response workflows (the fix playbooks)
- Calling an external LLM API (Gemini) from client-side JavaScript, and the security tradeoffs of doing so

---

## 🤝 Contributing

Feel free to fork and extend FaultGuard:
- Add a **network latency** metric
- Add **Chart.js** graphs for historical trend lines
- Add a **dark/light mode toggle**
- Connect to a real backend via **WebSockets** or the `/proc` filesystem (Linux)
- Swap in a different LLM provider for fault diagnosis (e.g. Claude, via a small backend proxy)

---

## 📄 License

MIT — free to use, modify, and share for educational purposes.

---

*Made with ⚡ for students learning system monitoring and web development.*
