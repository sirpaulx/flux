# 🚀 Renovate + Flux GitOps Automation (Combined Workflow)

This repository uses **Renovate Bot** with **FluxCD GitOps** to keep dependencies updated automatically — all managed by a **single GitHub Actions workflow**.

The automation:
- Scans dependencies and manifests (Helm charts, Docker images, Flux resources)
- Opens Pull Requests for updates
- Reconciles Flux after updates are merged
- Sends Slack notifications when PRs are created, merged, or closed

---

## 📌 **Workflow File**
```
.github/workflows/renovate-flux-combined.yml
```
This single workflow handles:
1. **Scheduled Renovate runs** + Flux reconciliation
2. **PR alerts** when Renovate opens, merges, or closes a PR

---

## ⚙️ **How It Works**

### **1️⃣ Renovate Job (Scheduler)**
- **Trigger:** Runs every Monday at 3AM UTC, or manually from GitHub Actions.
- **Steps:**
  1. Checks out the repo
  2. Installs Node.js and Renovate CLI
  3. Installs Flux CLI
  4. Runs Renovate scan based on `renovate.json` rules
  5. After successful scan, runs `flux reconcile` to apply updates

---

### **2️⃣ Alerts Job**
- **Trigger:** Runs on Pull Request events (`opened`, `reopened`, `closed`).
- **Steps:**
  1. Detects PR status (Opened, Merged, Closed without merge)
  2. Sends a Slack notification to the configured channel with:
     - Status icon (🆕, ✅, ❌)
     - PR title
     - Link to PR

---

## 📂 **Repository Structure**
```
.
├── renovate.json                          # Renovate configuration (update rules, schedules)
└── .github/
    └── workflows/
        └── renovate-flux-combined.yml     # Combined workflow (Renovate + Flux + Alerts)
```

---

## 🛠 **Secrets & Variables Required**

### **GitHub Secrets**
| Secret | Description |
|--------|-------------|
| `RENOVATE_GITHUB_TOKEN` | GitHub Personal Access Token or App token for Renovate authentication |
| `SLACK_WEBHOOK_URL`     | Slack or MS Teams webhook URL for sending PR notifications |

---

## 📝 **Environment Variables Used**
- **Inside Renovate job:**
  - `RENOVATE_TOKEN`: Authentication for Renovate to create PRs
  - `LOG_LEVEL`: Logging detail (set to `info` or `debug` for troubleshooting)
- **Inside Alerts job:**
  - `SLACK_WEBHOOK_URL`: Webhook endpoint for Slack/MS Teams notifications

---

## 📅 **Schedule**
- **Renovate scan:**  
  Runs every Monday at `03:00 UTC` (configurable in `cron` expression)
- **Manual trigger:**  
  Can be run anytime from GitHub Actions → *Run Workflow*
- **Alerts:**  
  Sent in real-time on PR creation, merge, or close

---

## 🔍 **General Idea**
This setup automates dependency management:
1. **Renovate scans the repo** (Flux manifests, Helm values, Docker images)
2. **PRs are created** based on rules in `renovate.json`
   - Minor/Patch updates → Auto-merge (for dev/staging)
   - Major updates → Manual review
   - Security updates → High priority
3. **After PR merge**, `flux reconcile` applies changes to the cluster
4. **Alerts** are sent to Slack/MS Teams to keep the team informed

---

## 📊 **Benefits**
✅ Fully automated dependency updates  
✅ Immediate Flux reconciliation after updates  
✅ Environment-specific update rules (staging auto, prod manual)  
✅ Real-time PR notifications to the team  
✅ Single file for easier maintenance  

---

## 🔗 **References**
- [Renovate Bot Documentation](https://docs.renovatebot.com/)
- [FluxCD Documentation](https://fluxcd.io/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
