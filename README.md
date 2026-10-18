# Render keep-alive bot

Render's free tier sleeps a web service after 15 minutes without traffic.
This repo's GitHub Actions workflow (`.github/workflows/keepalive.yml`) sends a
request to the service every 10 minutes so it never idles long enough to sleep.

**Keep this repo public.** Public repos get unlimited free Actions minutes. A job
every 10 minutes uses about 4,300 minutes a month, and a private repo gets only 2,000 free.

## Setup

1. Create an empty **public** GitHub repo, e.g. `render-keepalive`.
2. From this folder:
   ```
   git init -b main
   git add .
   git commit -m "Keep-alive bot"
   git remote add origin https://github.com/<you>/render-keepalive.git
   git push -u origin main
   ```
3. In the repo, go to **Settings → Secrets and variables → Actions → Variables**
   and add the variable `RENDER_URLS`, e.g.
   `https://<your-app>.onrender.com/healthz`.
   For more than one service, separate the URLs with commas.
4. On the **Actions** tab, open **Render keep-alive** and click **Run workflow**. The
   log should show `OK <url> -> 200 …s`.

## Notes

- Scheduled runs can start a few minutes late when GitHub is busy. A 10-minute
  interval leaves room for that inside Render's 15-minute window.
- If a ping fails, the run turns red and GitHub emails you.
- `heartbeat.txt` gets a commit about once a week. GitHub disables scheduled workflows
  in public repos after 60 days with no activity, and the commit prevents that.
