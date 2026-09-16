# Updating your site + turning on hourly automation

This is one bundle covering everything: the updated site files (now
showing the exact time of the last update, not just the date) and the
hourly auto-refresh setup. Do this in order.

## Part 1 — Update your existing site files

1. In your `crude-flow-dashboard` repo on GitHub, click **Add file →
   Upload files**.
2. Drag in `index.html`, `service-worker.js`, and `data.json` from this
   bundle — they replace the versions already there. (`manifest.json` and
   the `icons/` folder are unchanged from before; you can upload them too,
   it'll just say there's nothing to change.)
3. Commit.

That alone gets you the new "Live · updated 16 Sep, 14:03"-style time
display — even before the automation below is switched on, since it also
applies to the copy of the data already in your repo.

## Part 2 — Turn on hourly automation

This makes the site refresh itself every hour — Claude runs on GitHub's
own servers, checks for real developments, and pushes `data.json` itself
when something's actually changed. Cost: this draws on your existing Claude
Pro subscription's usage allowance, same pool as your normal chats — not a
separate bill.

Since you deleted the earlier personal access token, this uses a different
(and better-suited) mechanism entirely — a token really has nothing to do
with this approach, so there's nothing to recreate from before.

### Step 1 — Generate a Claude Code subscription token

1. Open the Claude desktop app → the **Code** tab (next to Chat and
   Cowork).
2. Start a session with **Environment: Local** (any folder is fine).
3. Open its integrated terminal and run:

   ```
   claude setup-token
   ```

4. It opens your browser to confirm you're authorizing your own Pro
   subscription, then prints a token in the terminal. Copy it.

### Step 2 — Install the Claude GitHub App on your repo

1. Go to **github.com/apps/claude** → **Install**.
2. **Only select repositories** → pick `crude-flow-dashboard` → **Install**.

### Step 3 — Add the token as a repository secret

1. In your repo → **Settings** → **Secrets and variables** → **Actions**.
2. **New repository secret**.
3. Name: `CLAUDE_CODE_OAUTH_TOKEN`
4. Value: paste the token from Step 1.
5. **Add secret**.

### Step 4 — Add the automation files

Upload these two files from this bundle, keeping the folder structure:

- `CLAUDE.md` → repo **root** (next to `index.html`)
- `.github/workflows/hourly-data-refresh.yml` → a `.github/workflows/`
  folder (GitHub's upload will create the folders if you drop in the whole
  path, or use **Add file → Create new file** and type the path with
  slashes)

Commit.

### Step 5 — Test it without waiting an hour

1. Repo → **Actions** tab → **Hourly crude flow data refresh** (left list).
2. **Run workflow** → **Run workflow** to confirm.
3. Watch it run (a couple of minutes). Check `data.json`'s commit history
   afterwards to see whether it found anything and updated the timestamp.

If it fails, click into the run for the error — usually the secret name
being slightly off, or the GitHub App not having access to this repo yet.

## What changed from the original weekly plan

Both this website and the Claude artifact version now check **hourly**
instead of weekly, and each run is designed to be fast — a quick check
first, with a fuller research pass only when something real turns up — so
most hourly runs do very little work. Still, hourly is a much bigger
request volume than weekly (roughly 24-168x), drawing on your normal
Claude usage both here and on the artifact's own hourly Cowork refresh. If
that ever feels like too much — noise, usage, or otherwise — turning it
back down is just editing the `cron:` line in
`.github/workflows/hourly-data-refresh.yml` (for the website) or asking me
to adjust the Cowork schedule (for the artifact); no rebuild needed either
way.

## Reminder

You already have your public URL live from the first deploy — none of this
changes that address, it's the same site, just refreshing itself now
instead of waiting for a manual upload.
