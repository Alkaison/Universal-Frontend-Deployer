# 🛠️ Frontend Deployment Standard

All frontend applications must follow the structure and guidelines below to ensure consistency, security, and automation readiness.

- Complete setup instructions are [here](./SETUP.md "Setup File").
- Slack Incoming Webhooks setup guide is [here](https://api.slack.com/messaging/webhooks).

## 📁 Folder Structure

- All frontend repositories must be cloned to:

```yaml
/home/<username>/<project-name>/
```

- The application is built and served from this location.

```yaml
/home/<username>/<project-name>/dist/
```

> We are considering the default build folder to be named as "dist", if it's different in your case, replace every instance of "dist" to the build folder name of yours as per the project setup.

## ✅ Prerequisites

### Server Setup

- Ubuntu 20.04+ (recommended)
- SSH access with password or private key authentication
- Web server installed (e.g., **nginx**)
- Node.js and npm installed **globally** on the system
- Optional: PM2 for SSR/Next.js (if needed)

> ℹ️ Ensure Node.js and npm are installed using [NodeSource](https://github.com/nodesource/distributions) or your preferred method.

> Use the same version used by the frontend team for local builds. Ask them directly if unsure.

## 📦 Project Standards

### Required Files

Each repository **must** include:

- `package.json`
- `package-lock.json`
- `.gitignore` (with `dist/` ignored)
- Proper build scripts

### Package.json Standards

All frontend projects must define these scripts:

```json
"scripts": {
  "build": "vite build",
  "dev": "vite",
  "preview": "vite preview"
}
```

Other frameworks like Next.js can override with:

```json
"build": "next build && next export"
```

## 🔧 Breakdown of Script Modules (Reusable Blocks)

1. `pull_and_build.yml`

Purpose: Pull the latest code on the server and build the frontend.

Tasks:

- SSH into the server
- Pull from GitHub or re-clone cleanly
- Run `npm ci` and `npm run build`
- Move the existing `dist/` to `dist_prev/`
- Build a new app into `dist/`
- Reload nginx to serve the updated files

2. `rollback.yml`

Purpose: Restore the last known good build if anything fails (e.g., broken deployment, failed health check).

Tasks:

- Delete the broken `dist/`
- Move `dist_prev/` back to `dist/`
- Reload nginx

3. `slack_alert_and_healthcheck.yml`

Purpose:

- Ping the deployed URL (via `curl` or `http_status`)
- Send a Slack message using a webhook

Scenarios:

- ✅ If the site is healthy → send a **"Deployed Successfully"** message
- ❌ If the site fails health check:
  - Trigger `rollback.yml`
  - Send a **"Deployment Failed. Rolled back."** alert

## 🧩 Main Controller Workflow (`deploy.yml`)

This is the central orchestrator for the deployment pipeline.

**Steps:**

1. Triggered on push to `<branch_name>` (or manual dispatch)
1. Calls `pull_and_build.yml`
1. Calls `slack_alert_and_healthcheck.yml`
1. If health check fails, runs `rollback.yml`
1. Slack alert is always sent (on success or failure)

## 📡 Slack Alerts

All deployment jobs send a Slack message to the configured webhook.

You will receive:

- ✅ Success notifications
- ❌ Failure notifications (including cause and rollback confirmation)

Make sure the `SLACK_WEBHOOK_URL` is added to the repo secrets.

## 💬 Need Help?

Contact [Alkaison](https://github.com/Alkaison) for help in setup or further improvements as per your project.
