# Teramot Dashboard Deployer

A zero-infrastructure static app that lets you deploy AI-generated HTML dashboards directly to GitHub Pages — no backend, no server, just your browser talking to the GitHub API.

## How it works

- You provide a GitHub Personal Access Token and a destination repo
- The app stores both in your browser's `localStorage` (never sent anywhere except GitHub)
- Uploading a dashboard does a `PUT` to the GitHub Contents API — same as committing a file
- GitHub Pages serves the result at `https://{owner}.github.io/{repo}/dashboards/{slug}/index.html`

---

## Setup

### 1. Create the GitHub repo

Create a public repo (or private if your plan supports private Pages). This same repo hosts both the deployer app and the deployed dashboards.

### 2. Enable GitHub Pages

1. Go to **Settings → Pages** in your repo
2. Set **Source** to `Deploy from a branch`
3. Set **Branch** to `main`, folder `/` (root)
4. Click **Save**

GitHub Pages will be available at `https://{owner}.github.io/{repo}/`.

### 3. Generate a GitHub Token

1. Go to **GitHub → Settings → Developer settings → Personal access tokens → Fine-grained tokens**
2. Click **Generate new token**
3. Set a name (e.g. "Dashboard Deployer") and expiration
4. Under **Repository access**, select **Only select repositories** → choose this repo
5. Under **Repository permissions**, set:
   - **Contents**: Read and write
6. Click **Generate token** and copy it immediately (you won't see it again)

### 4. Deploy the app

Push this repo to `main`. Once GitHub Pages is active, open:

```
https://{owner}.github.io/{repo}/
```

On first visit, enter your token and repo name (`owner/repo`). The app verifies access and saves the config.

---

## Usage

**Deploy tab** — Drag & drop an HTML file → preview it → give it a name → click Deploy.

**Dashboards tab** — See all deployed dashboards with options to copy the URL, open it, re-deploy (update the file), or delete.

**Settings** (gear icon) — Update your token or repo, or reset all settings.

---

## Repo structure

```
/
├── index.html          ← the deployer app (this file)
├── README.md
└── dashboards/
    ├── {slug}/
    │   └── index.html  ← deployed dashboard
    └── ...
```

The deployer app and dashboards live in the same repo. GitHub Pages serves everything from `main`.

---

## Troubleshooting

| Issue | Fix |
|---|---|
| "Repo not found" | Check `owner/repo` spelling and that the token has access |
| 401 Unauthorized | Token may be expired or missing Contents permission |
| Dashboard URL returns 404 | GitHub Pages takes 30–60s after first deploy; wait and refresh |
| Large HTML fails | GitHub API has a 100MB limit per file; HTML dashboards are always well under this |
