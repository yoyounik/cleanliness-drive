# cleanliness-drive
AI-powered civic cleanliness reporting system using Coral MCP

# 🧹 Cleanliness Drive — AI-Powered Civic Reporting System

> Built for the **Pirates of the Coral-bean Hackathon** by WeMakeDevs  
> Powered by [Coral](https://withcoral.com) — query any data source as SQL, zero ETL

---

## 🌍 Problem Statement

Cities like Delhi face massive cleanliness challenges — but there's no easy way for citizens to report issues AND for authorities to track, query, and act on them across multiple data sources in real time.

**Cleanliness Drive** solves this by connecting citizen complaint data (Google Forms) with GitHub issue tracking — all queryable via a single SQL interface powered by Coral MCP and Claude AI.

---

## 🏗️ Architecture

```
Google Form (Citizen fills complaint)
        ↓
Google Sheets (Auto stores responses)
        ↓
CSV Export (Downloaded locally)
        ↓
Coral Source (YAML config, file-backed)
        ↓
Coral SQL Engine ←→ GitHub Issues
        ↓
Claude Desktop (via MCP)
        ↓
AI-powered cross-source queries & insights
```

---

## ✨ Features

- 📝 **Citizen Reporting** — Simple Google Form for reporting cleanliness issues with location and severity
- 🗃️ **SQL over CSV** — Coral turns raw CSV form data into a queryable SQL table instantly, no database needed
- 🐙 **GitHub Integration** — Cross-source JOIN between citizen reports and GitHub issues tracking
- 🤖 **AI Queries via MCP** — Claude Desktop connects to Coral over MCP and answers natural language questions about your data
- ⚡ **Zero ETL** — No data warehouse, no pipeline, no glue code — just Coral

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| [Coral](https://withcoral.com) | SQL engine over CSV + GitHub |
| Google Forms + Sheets | Citizen complaint collection |
| Coral MCP | Exposes Coral to Claude as tools |
| Claude Desktop | AI querying over MCP |
| GitHub | Issue tracking for complaints |
| PowerShell | CLI for running Coral commands |

---

## 🚀 Setup & Installation

### Prerequisites
- [Coral CLI](https://withcoral.com/docs/getting-started/installation) installed
- Claude Desktop installed
- GitHub account

### Step 1 — Clone this repo
```bash
git clone https://github.com/yoyounik/cleanliness-drive
cd cleanliness-drive
```

### Step 2 — Download your Google Sheet as CSV
In Google Sheets → **File → Download → Comma Separated Values (.csv)**  
Save it as `responses.csv` in the project folder.

### Step 3 — Add Cleanliness Drive as a Coral source
```powershell
coral source add --file ".\cleanliness-drive.yaml"
coral source test cleanliness_drive
```

### Step 4 — Add GitHub as a Coral source
```powershell
coral source add --interactive github
```
Follow the device code auth flow to connect your GitHub account.

### Step 5 — Connect Claude Desktop via MCP
Add to `%APPDATA%\Claude\claude_desktop_config.json`:
```json
{
  "mcpServers": {
    "coral": {
      "command": "C:\\Users\\YourName\\.local\\bin\\coral.exe",
      "args": ["mcp-stdio"]
    }
  }
}
```
Restart Claude Desktop — look for the 🔨 hammer icon.

---

## 🔍 Example SQL Queries

### View all citizen complaints
```sql
SELECT * FROM cleanliness_drive.reports
```

### Filter by severity
```sql
SELECT * FROM cleanliness_drive.reports 
WHERE text = 'High'
```

### Cross-source JOIN — complaints + GitHub issues
```sql
SELECT r."TimeStamp", r.text, i.title AS github_issue, i.state 
FROM cleanliness_drive.reports r 
CROSS JOIN github.issues i 
WHERE i.owner = 'yoyounik' AND i.repo = 'cleanliness-drive'
```

### List all GitHub tracking issues
```sql
SELECT number, title, state 
FROM github.issues 
WHERE owner = 'yoyounik' AND repo = 'cleanliness-drive'
```

---

## 🤖 Ask Claude (via MCP)

Once Claude Desktop is connected, you can ask:

> *"Show me all High severity cleanliness complaints"*

> *"Which locations have reported cleanliness issues?"*

> *"Cross-reference citizen complaints with open GitHub issues"*

> *"Summarize all cleanliness reports and their severity levels"*

---

## 📁 Project Structure

```
cleanliness-drive/
├── cleanliness-drive.yaml   # Coral source spec for CSV data
├── responses.csv            # Citizen complaint data from Google Form
└── README.md                # You are here
```

---

## 📸 Screenshots

> 💡 **Add these screenshots to make your README shine:**
> 1. Google Form as seen by citizens
<img width="923" height="749" alt="image" src="https://github.com/user-attachments/assets/49ed7cbc-3bc2-4f51-bbe2-c6f9b9a77323" />

> 2. Google Sheet with responses
<img width="830" height="411" alt="image" src="https://github.com/user-attachments/assets/500bff71-f763-408a-acd6-6f1a9bb57d59" />


---

## 🗺️ Roadmap

- [ ] Auto-sync Google Sheets without manual CSV export (Google Sheets API custom source)
- [ ] Add more form fields — photo upload, ward number, category
- [ ] Dashboard UI showing complaints on a city map
- [ ] Slack integration — notify authorities when High severity issues are reported
- [ ] Automated GitHub issue creation when a new complaint is submitted

---

## 👨‍💻 Author

Built by **Nikhil** ([@yoyounik](https://github.com/yoyounik)) for the Pirates of the Coral-bean Hackathon 🏴‍☠️

---

## 📄 License

MIT
