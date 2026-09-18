# Putting the Global Crude Flow dashboard on the web — step by step

You don't need to know how to code or use a terminal for this. Everything
below is done by clicking around two free websites.

## What you're deploying

This zip contains a complete, self-contained website:

- `index.html` — the app itself (works standalone, no build step)
- `manifest.json`, `service-worker.js`, `icons/` — what makes it installable
  as an app on a phone or laptop ("Add to Home Screen")
- `data.json` — the data the dashboard reads. Updating this file each week
  is how you refresh the live site without re-uploading everything.

## Step 1 — Create a free GitHub account

1. Go to **github.com** and click **Sign up**.
2. Use any email address, pick a username, set a password. Verify your
   email when it asks.

(GitHub is free for this — public sites cost nothing, no credit card.)

## Step 2 — Create a new repository

1. Once logged in, click the **+** in the top-right corner → **New repository**.
2. Name it something like `crude-flow-dashboard`.
3. Leave it set to **Public** (required for the free hosting).
4. Tick **"Add a README file"**.
5. Click **Create repository**.

## Step 3 — Upload the website files

1. On your new repository's page, click **Add file → Upload files**.
2. Unzip `global-crude-flows-webapp.zip` on your computer first, then drag
   in everything **inside** the unzipped folder — `index.html`,
   `manifest.json`, `service-worker.js`, `data.json`, and the whole `icons`
   folder — so they land in the root of the repository (not inside an extra
   subfolder).
3. Scroll down, click **Commit changes**.

## Step 4 — Turn on GitHub Pages

1. In your repository, click **Settings** (top menu).
2. In the left sidebar, click **Pages**.
3. Under "Build and deployment" → "Source", choose **Deploy from a branch**.
4. Under "Branch", choose **main** and folder **/ (root)**, then **Save**.
5. Wait about a minute, then refresh the page. GitHub will show you your
   live URL — something like:

   `https://yourusername.github.io/crude-flow-dashboard/`

That's your public website. Open it — it should look identical to the
dashboard you've been using in Claude.

## Step 5 — Install it as an app

- **On a phone (iPhone):** open the link in Safari → tap the Share icon →
  **Add to Home Screen**.
- **On a phone (Android):** open the link in Chrome → tap the ⋮ menu →
  **Add to Home screen** / **Install app**.
- **On a laptop (Chrome/Edge):** open the link → look for an install icon
  (⊕ or a small monitor icon) in the address bar → **Install**.

It'll then open in its own window with its own icon, just like a normal
app — and it keeps working even with no signal, showing the last data it
managed to load.

## Keeping it up to date

The site re-checks `data.json` every time it's opened (and again if you
go back online after being offline), so refreshing the dashboard is just
replacing that one file:

1. Get the new `data.json` (Claude can generate this in your weekly
   refresh, same as it does for the version in the Claude project).
2. In your GitHub repository, click on `data.json` → the pencil (✎) **Edit**
   icon → **or** use **Add file → Upload files** and drop in the new
   `data.json` to overwrite it → **Commit changes**.
3. That's it — no need to re-upload `index.html` or anything else unless
   the dashboard's design itself changes. Anyone with the site open, or who
   opens it later, will pick up the new data automatically within a few
   seconds.

If you'd like this last step automated too (so Claude pushes the weekly
update straight to GitHub instead of you uploading it by hand), that's
possible with a GitHub "personal access token" — just ask, and I'll walk
you through creating one and wire it into the weekly scheduled refresh.
That does mean a Claude-held credential with write access to this one
repository, which is worth weighing against five minutes of manual
uploading once a week.
