# Deploying to GitHub Pages

## Prerequisites
- Git installed on your machine
- A GitHub account at github.com/equinox013
- The repo name must be exactly `equinox013.github.io`

---

## Step 1 — Create the repo on GitHub

1. Go to https://github.com/new
2. Repository name: `equinox013.github.io`
3. Set it to **Public**
4. Do NOT tick "Add a README" — the folder already has files
5. Click **Create repository**

---

## Step 2 — Push from your local machine

Open **Git Bash** (or any terminal) and run these commands one at a time:

```bash
# Navigate to the folder
cd "D:\Git Repos\equinox013.github.io"

# Initialise git
git init

# Stage everything
git add .

# First commit
git commit -m "feat: initial portfolio site launch"

# Point to your GitHub repo
git remote add origin https://github.com/equinox013/equinox013.github.io.git

# Push
git push -u origin main
```

> If git asks for credentials, sign in with your GitHub username and a **Personal Access Token**
> (not your password). Generate one at: GitHub → Settings → Developer Settings → Personal Access Tokens → Tokens (classic) → Generate new token → tick `repo` scope.

---

## Step 3 — Enable GitHub Pages

1. Go to your repo: https://github.com/equinox013/equinox013.github.io
2. Click **Settings** (top tab)
3. Left sidebar → **Pages**
4. Under "Branch", select `main` and folder `/ (root)`
5. Click **Save**

GitHub will show: *"Your site is live at https://equinox013.github.io"*

Allow 1–3 minutes for the first deployment to propagate.

---

## Making updates later

```bash
cd "D:\Git Repos\equinox013.github.io"
git add .
git commit -m "update: <describe your change>"
git push
```

GitHub Pages auto-redeploys on every push to main. Changes typically go live within 60 seconds.

---

## Swapping the profile photo

Replace `assets/profile.jpg` with any new photo (keep the same filename), then push.
If you want a different filename, update line ~288 in `index.html`:
```html
<img src="assets/profile.jpg" alt="Nicholas Dale" class="hero-photo" />
```
