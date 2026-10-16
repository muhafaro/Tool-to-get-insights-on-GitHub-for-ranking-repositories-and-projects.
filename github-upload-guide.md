# GitHub upload guide — three separate projects

This guide publishes each project as its **own GitHub repository**. Do not put all three folders in one repository and do not upload the ZIP files themselves. Extract a ZIP first, then upload the extracted project folder.

Projects:

| Project | Folder | Suggested GitHub repository name |
| --- | --- | --- |
| GitHub Portfolio Insights Dashboard | `gitpulse-dashboard` | `gitpulse-dashboard` |
| Home Energy Monitor | `home-energy-monitor` | `wattwatch-home-energy-monitor` |
| Cybersecurity Scanner CLI | `sentinelguard-cli` | `sentinelguard-cli` |

---

## 1. One-time setup

### Install the tools

You need:

- A GitHub account
- Git: run `git --version` in Terminal to confirm it is installed
- Python 3.10 or later: run `python3 --version`

### Configure Git identity once

Use your real name and the email associated with your GitHub account:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

### Make three empty GitHub repositories

On GitHub, click **+** → **New repository**. Create these one at a time:

1. `gitpulse-dashboard`
2. `wattwatch-home-energy-monitor`
3. `sentinelguard-cli`

For every repository:

- Owner: your account
- Visibility: **Public** for a portfolio project, or Private while you are still editing
- Do **not** tick “Add a README”, “Add .gitignore”, or “Choose a license”; the folders already contain these files or will receive them below
- Click **Create repository**
- Copy the HTTPS repository URL GitHub shows, for example: `https://github.com/YOUR_USERNAME/gitpulse-dashboard.git`

---

## 2. Final pre-upload checks

Run these commands **inside the relevant project folder**. They create a local virtual environment, install dependencies, run the tests, and start the app/CLI check. Do not upload the `.venv` folder; each project’s `.gitignore` excludes it.

### A. GitPulse dashboard

```bash
cd /PATH/TO/gitpulse-dashboard
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt pytest
pytest -q
uvicorn app.main:app --host 127.0.0.1 --port 8000
```

Open `http://127.0.0.1:8000`, analyze a public GitHub username, then stop the server with `Ctrl+C`.

Check that `.env` does not exist or contains no real token. If you use a token locally, it must remain only in `.env`, never in source code.

### B. WattWatch home energy monitor

```bash
cd /PATH/TO/home-energy-monitor
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt pytest
pytest -q
uvicorn app.main:app --host 127.0.0.1 --port 8000
```

Open `http://127.0.0.1:8000`. Confirm the seeded dashboard loads and that importing a small test CSV works. Stop with `Ctrl+C`.

If you imported actual household readings, delete `data/energy.db` before upload so your private energy data is not published. The app will automatically create fresh demo data on the next run.

### C. SentinelGuard cybersecurity scanner

First replace the placeholder author text before publishing:

```bash
cd /PATH/TO/sentinelguard-cli
```

Open `LICENSE` and replace `Your Name` with your name. In `pyproject.toml`, replace `Your Name` in the `authors` entry too.

Then test it:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -e . pytest
pytest -q
sentinelguard scan . --fail-on none
```

Expected outcome: `No findings`. Do not publish SentinelGuard until it reports no findings when scanning itself.

---

## 3. Security and quality checklist

Complete this checklist separately for every project.

- [ ] I tested the project locally using the section above.
- [ ] `git status` does not show `.env`, `.venv`, databases, or personal files.
- [ ] I searched the folder for real passwords, API keys, tokens, private keys, and personal data.
- [ ] The README has a clear project description, installation steps, and screenshots or demo details where appropriate.
- [ ] The app starts successfully, or the CLI runs successfully.
- [ ] I reviewed the project name and repository description for spelling.
- [ ] I have not used copyrighted logos, screenshots, or datasets that I do not have permission to publish.
- [ ] I added the right license.
- [ ] I understand that `git push` will make a public repository publicly visible.

Run this final Git check immediately before committing:

```bash
git status
git diff --cached
```

`git status` should list only files that belong in the project. `git diff --cached` lets you inspect what will be published before the commit.

---

## 4. Upload with Terminal (recommended)

Repeat this complete block **one project at a time**. Replace the folder path and URL for the project you are uploading.

```bash
cd /PATH/TO/PROJECT-FOLDER
git init
git add .
git status
git diff --cached
git commit -m "Initial release"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
git push -u origin main
```

When GitHub asks you to authenticate, finish the browser sign-in or use a personal access token if prompted for a password.

### Exact examples

Use these only after replacing `YOUR_USERNAME`.

```bash
# GitPulse
cd /PATH/TO/gitpulse-dashboard
git init
git add .
git commit -m "Initial release"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/gitpulse-dashboard.git
git push -u origin main
```

```bash
# WattWatch
cd /PATH/TO/home-energy-monitor
git init
git add .
git commit -m "Initial release"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/wattwatch-home-energy-monitor.git
git push -u origin main
```

```bash
# SentinelGuard
cd /PATH/TO/sentinelguard-cli
git init
git add .
git commit -m "Initial release"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/sentinelguard-cli.git
git push -u origin main
```

If `git remote add origin` says that `origin` already exists, check it with:

```bash
git remote -v
```

If it is the wrong URL, correct it with:

```bash
git remote set-url origin https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
```

---

## 5. Upload with the GitHub website (no Terminal)

Use this only for small projects. The Terminal method is better because it preserves normal Git history.

1. Create the correct empty repository from section 1.
2. Open it on GitHub and select **uploading an existing file**.
3. Open the project folder on your computer.
4. Select the project files and folders inside it—not the enclosing folder and not the `.zip` file.
5. Do not select `.venv`, `.env`, `data/energy.db`, `__pycache__`, or any personal CSV.
6. Write `Initial release` as the commit message and click **Commit changes**.
7. Open the README on GitHub and check that the file tree and license display correctly.

---

## 6. After upload: GitHub checks

For each repository:

- [ ] Open the repository in an incognito/private browser window to see exactly what others can see.
- [ ] Confirm the README renders correctly.
- [ ] Check the **Actions** tab; the CI workflow should pass after the first push.
- [ ] Check the **Code** tab; no `.env`, `.venv`, `energy.db`, or real data should be visible.
- [ ] Add a concise repository description and topics.
- [ ] Pin your best one or two projects to your GitHub profile.

Suggested topics:

| Repository | Topics |
| --- | --- |
| GitPulse | `python`, `fastapi`, `github-api`, `dashboard`, `portfolio` |
| WattWatch | `python`, `fastapi`, `energy-monitoring`, `sqlite`, `data-visualization` |
| SentinelGuard | `python`, `cybersecurity`, `cli`, `devsecops`, `sarif` |

## 7. License action before publishing

SentinelGuard already has an MIT license; personalize the copyright holder as described above. GitPulse and WattWatch should receive an MIT `LICENSE` file before publication if you want others to reuse them. Without a license, others do not receive permission to reuse your code.

Do not claim an affiliation with GitHub, AWS, Slack, Docker, FastAPI, or any energy supplier. Their names may be used descriptively in documentation.
