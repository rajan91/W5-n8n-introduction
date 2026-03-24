# 🔄 n8n — Workflow Automation Platform

> A comprehensive guide to understanding, setting up, and using n8n for workflow automation.

---

## Table of Contents

1. [What is n8n?](#1-what-is-n8n)
2. [What is n8n Used For?](#2-what-is-n8n-used-for)
3. [n8n Platform Setup Types](#3-n8n-platform-setup-types)
   - [Self-Host](#31-self-host)
   - [Local Setup](#32-local-setup)
   - [n8n Production](#33-n8n-production)
4. [n8n Canvas Editor](#4-n8n-canvas-editor)
5. [n8n Nodes](#5-n8n-nodes)
6. [Sticky Notes](#6-sticky-notes)

---

## 1. What is n8n?

**n8n** (pronounced *"nodemation"*) is a **free and open-source workflow automation tool** that allows you to connect different apps, services, and APIs to automate repetitive tasks — without needing to write extensive code.

It follows a **node-based, visual programming approach** where each action or integration is represented as a "node" on a canvas, and you connect nodes together to build automation workflows.

> 💡 **Key Fact:** n8n is **source-available** and can be **self-hosted**, giving teams full control over their data and infrastructure — unlike many SaaS automation tools.

### Core Characteristics

- Visual, drag-and-drop workflow builder
- 400+ built-in integrations (Slack, GitHub, Google Sheets, databases, etc.)
- Supports custom JavaScript/Python code within workflows
- Can be self-hosted or used via n8n Cloud
- Supports complex logic: branching, looping, error handling

---

## 2. What is n8n Used For?

n8n is used to **automate business processes** by connecting multiple tools and services into a single automated pipeline.

### Common Use Cases

| Category | Example |
|---|---|
| **Data Sync** | Sync CRM contacts to a database automatically |
| **Notifications** | Send Slack alerts when a GitHub issue is created |
| **ETL Pipelines** | Extract data from an API, transform it, load into a spreadsheet |
| **Lead Management** | Auto-qualify form submissions and add to CRM |
| **DevOps Automation** | Trigger deployments based on webhook events |
| **AI Workflows** | Chain LLM calls with external tools and APIs |
| **Reporting** | Generate and email daily reports from database queries |
| **Customer Support** | Auto-respond to tickets based on keywords |

### Who Uses n8n?

- **Developers** — Build complex integrations with custom code nodes
- **Business Analysts** — Automate reporting and data pipelines
- **DevOps/IT Teams** — Infrastructure and monitoring automations
- **Marketing Teams** — Lead nurturing, campaign triggers
- **Startups** — Rapid prototyping of product integrations

---

## 3. n8n Platform Setup Types

n8n can be deployed and run in several ways depending on your needs.

```
┌─────────────────────────────────────────────┐
│             n8n Deployment Options           │
├──────────────┬──────────────┬────────────────┤
│  Local Setup │  Self-Host   │  n8n Cloud     │
│  (Dev/Test)  │ (Full Control│  (Managed SaaS)│
└──────────────┴──────────────┴────────────────┘
```

---

### 3.1 Self-Host

Self-hosting means **running n8n on your own server or infrastructure** — giving you complete control over data, security, and scaling.

#### Options for Self-Hosting

| Method | Description |
|---|---|
| **Docker** | Most popular; use the official `n8nio/n8n` image |
| **Docker Compose** | Multi-container setup with database and n8n |
| **Kubernetes** | Enterprise-scale, highly available deployments |
| **VPS/Cloud VM** | Run directly on AWS EC2, DigitalOcean, GCP, etc. |

#### Quick Docker Example

```bash
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

#### Self-Host Advantages

- ✅ Full data privacy and control
- ✅ No usage-based pricing
- ✅ Custom environment configuration
- ✅ Integrate with internal/private services
- ⚠️ You are responsible for updates, backups, and uptime

---

### 3.2 Local Setup

Local setup is used for **development, testing, and learning** purposes on your personal machine.

#### Prerequisites

- **Node.js** v18 or higher
- **npm** or **pnpm**

#### Installation via npm

```bash
# Install n8n globally
npm install n8n -g

# Start n8n
n8n start
```

#### Access

Once started, open your browser and navigate to:

```
http://localhost:5678
```

#### Local Setup Use Cases

- Learning n8n workflow building
- Testing new nodes and integrations
- Developing custom nodes
- Prototyping automations before deploying to production

> ⚠️ **Note:** Local setup is **not recommended for production**. It lacks persistence, SSL, and high-availability features.

---

### 3.3 n8n Production

A **production deployment** is a stable, secure, and highly available n8n instance used to run real business workflows.

#### Key Requirements for Production

| Requirement | Details |
|---|---|
| **Database** | PostgreSQL (recommended over default SQLite) |
| **Process Manager** | PM2 or Docker with restart policies |
| **SSL/TLS** | HTTPS via Nginx/Caddy reverse proxy |
| **Authentication** | Enable basic auth or SSO |
| **Backups** | Regular database and credential backups |
| **Monitoring** | Logs, health checks, alerting |

#### Docker Compose Production Example

```yaml
version: '3.8'
services:
  n8n:
    image: n8nio/n8n
    restart: always
    ports:
      - "5678:5678"
    environment:
      - DB_TYPE=postgresdb
      - DB_POSTGRESDB_HOST=postgres
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=securepassword
    volumes:
      - n8n_data:/home/node/.n8n
    depends_on:
      - postgres

  postgres:
    image: postgres:15
    environment:
      - POSTGRES_USER=n8n
      - POSTGRES_PASSWORD=n8npassword
      - POSTGRES_DB=n8n
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  n8n_data:
  postgres_data:
```

#### n8n Cloud (Managed Production)

If you don't want to manage infrastructure, **n8n Cloud** is the official managed SaaS option:

- Hosted and maintained by n8n team
- Automatic updates and backups
- Available at [app.n8n.cloud](https://app.n8n.cloud)

---

## 4. n8n Canvas Editor

The **Canvas Editor** is the main visual interface where you design and manage your workflows.

```
┌──────────────────────────────────────────────────────────────┐
│  n8n Canvas Editor                                           │
│                                                              │
│  ┌──────────┐     ┌──────────┐     ┌──────────┐            │
│  │ Trigger  │────▶│ Process  │────▶│  Output  │            │
│  │  Node    │     │  Node    │     │  Node    │            │
│  └──────────┘     └──────────┘     └──────────┘            │
│                                                              │
│  [+ Add Node]  [▶ Execute]  [💾 Save]  [⚙ Settings]        │
└──────────────────────────────────────────────────────────────┘
```

### Canvas Editor Components

| Component | Description |
|---|---|
| **Canvas Area** | Infinite scrollable whiteboard where nodes are placed |
| **Node Panel** | Left sidebar with searchable list of all available nodes |
| **Top Toolbar** | Save, Execute, Settings, Zoom controls |
| **Connections** | Lines/arrows connecting node outputs to inputs |
| **Mini-map** | Overview of large workflows for navigation |
| **Execution Log** | View last run data and debug errors |

### Canvas Interactions

- **Drag nodes** from the panel onto the canvas
- **Click a node** to open its configuration panel
- **Connect nodes** by dragging from output dot to input dot
- **Right-click** a node for quick actions (duplicate, delete, disable)
- **Ctrl/Cmd + scroll** to zoom in and out
- **Middle-click drag** or **space + drag** to pan the canvas

---

## 5. n8n Nodes

A **node** is the fundamental building block of any n8n workflow. Each node performs a specific action or represents a service/trigger.

### Node Types

| Type | Description | Example |
|---|---|---|
| **Trigger Nodes** | Start the workflow based on an event | Webhook, Cron, Email Trigger |
| **Action Nodes** | Perform an operation in a service | Send Email, Create Jira Ticket |
| **Data Nodes** | Transform or manipulate data | Set, Merge, Split In Batches |
| **Flow Nodes** | Control workflow logic | IF, Switch, Wait, Loop Over Items |
| **Code Nodes** | Execute custom JavaScript or Python | Code Node |
| **Core Nodes** | Low-level utilities | HTTP Request, Execute Command |

### Node Anatomy

```
┌────────────────────────────────┐
│  ● Input                       │  ◀── Data comes in here
│                                │
│  [Node Icon]  Node Name        │
│  Configured: Yes / No          │
│                                │
│  ● Output                      │  ──▶ Data passes out here
└────────────────────────────────┘
```

### Popular Built-in Nodes

- **HTTP Request** — Call any REST API
- **Webhook** — Receive HTTP requests as triggers
- **Schedule Trigger** — Run workflows on a cron schedule
- **IF** — Branch workflow based on conditions
- **Set** — Add, remove, or transform data fields
- **Code** — Write custom JavaScript/Python logic
- **Google Sheets** — Read/write spreadsheet data
- **Slack** — Send messages or read channels
- **Postgres / MySQL** — Query databases directly

### Custom Nodes

You can build and install **custom nodes** using n8n's node SDK:

```bash
# Create a new custom node project
npx n8n-node-dev new
```

---

## 6. Sticky Notes

**Sticky Notes** are annotation elements you can place on the canvas to **document, explain, and organize** your workflows.

### What Sticky Notes Do

- Add human-readable descriptions to workflow sections
- Warn collaborators about important logic or edge cases
- Group related nodes with visual labels
- Leave TODO notes for future development

### How to Add a Sticky Note

1. Right-click on an empty area of the canvas
2. Select **"Add Note"** (or press **N** as a shortcut)
3. A yellow sticky note appears — double-click to edit the text
4. Drag to reposition, resize from the corner handles

### Sticky Note Formatting

Sticky notes support **Markdown formatting**:

```markdown
## Section Title

This node fetches customer data from the CRM.

> ⚠️ API rate limit: 100 req/min

- Handles pagination automatically
- Returns up to 1000 records
```

### Best Practices for Sticky Notes

| Practice | Why |
|---|---|
| Label major workflow sections | Helps teammates navigate large workflows |
| Document non-obvious logic | Reduces debugging time |
| Add author + date on complex notes | Improves change tracking |
| Use warning notes for critical paths | Prevents accidental breakage |
| Keep notes close to relevant nodes | Maintains visual context |

---

## Quick Reference Summary

```
n8n
├── What it is        → Open-source visual workflow automation tool
├── What it does      → Connects apps and automates processes via nodes
├── Setup Types
│   ├── Local         → npm install n8n -g (dev/testing only)
│   ├── Self-Host     → Docker / VPS / Kubernetes (full control)
│   └── Production    → PostgreSQL + Docker Compose + SSL + Auth
├── Canvas Editor     → Visual whiteboard for building workflows
├── Nodes             → Building blocks: Trigger, Action, Logic, Code
└── Sticky Notes      → Markdown annotations for documentation
```

---

## Resources

- 📖 [Official Documentation](https://docs.n8n.io)
- 🎓 [n8n Community Forum](https://community.n8n.io)
- 🐙 [GitHub Repository](https://github.com/n8n-io/n8n)
- ☁️ [n8n Cloud](https://app.n8n.cloud)
- 📦 [Node Library](https://n8n.io/integrations)

---

*Last updated: March 2026*
