# The Codex — your campaign lore wiki

A small, self-hosted lore wiki: pages for Characters, Locations, Factions,
Items, and a Session Log, styled like a leather-bound campaign codex. It's
built with [Jekyll](https://jekyllrb.com/), which GitHub Pages runs for you
automatically — there's nothing to install and no build step you need to run.

## 1. Put this on GitHub

1. Go to [github.com/new](https://github.com/new) and create a new repository
   (any name — e.g. `campaign-codex`). Keep it **private** if you don't want
   strangers reading your lore, or public if you don't mind.
2. Upload everything in this folder to that repo. Easiest way with no command
   line: on the repo page, click **Add file → Upload files**, drag in every
   file and folder from here, and commit.
   (If you do use git: `git init`, `git add .`, `git commit -m "Initial codex"`,
   then `git remote add origin <your-repo-url>` and `git push -u origin main`.)
3. Open `_config.yml` in the repo and edit these three lines to match your repo:
   ```yaml
   github_username: "YOUR-GITHUB-USERNAME"
   github_repo: "YOUR-REPO-NAME"
   github_branch: "main"
   ```
   This just powers the "Edit this page on GitHub" link on every entry — it
   doesn't affect anything else.

## 2. Turn on GitHub Pages

1. In your repo, go to **Settings → Pages**.
2. Under "Build and deployment", set **Source** to **Deploy from a branch**,
   branch **main**, folder **/ (root)**. Save.
3. Wait a minute or two, then refresh — GitHub will show you the live URL,
   something like `https://your-username.github.io/your-repo-name/`.

Every time anyone commits a change, the live site rebuilds automatically in
under a minute. No rebuilding or redeploying by hand, ever.

## 3. Give people edit access

Go to **Settings → Collaborators → Add people**, and invite by GitHub
username or email. Once they accept, they can edit.

## 4. The actual "easy editor"

You don't need any special tool — GitHub's own web editor is the editor:

1. Open the file you want to change (or navigate to it from the folders
   below).
2. Click the pencil icon in the top right of the file view.
3. Edit the text — it's plain [Markdown](https://www.markdownguide.org/basic-syntax/),
   which is just normal writing with a few symbols (`##` for a heading, `- `
   for a bullet, `[link text](/some/url/)` for a link).
4. Scroll down, add a one-line commit message ("update Elyndra's backstory"),
   click **Commit changes**. The live site updates within a minute.

No git commands, no local setup — this works from a phone browser too.

## Adding a new page

Each type of page lives in its own folder as a `.md` file:

| Type      | Folder         | Example                                  |
|-----------|----------------|-------------------------------------------|
| Character | `_characters/` | `_characters/example-character.md`        |
| Location  | `_locations/`  | `_locations/example-location.md`          |
| Faction   | `_factions/`   | `_factions/example-faction.md`            |
| Item      | `_items/`      | `_items/example-item.md`                  |
| Session   | `_sessions/`   | `_sessions/session-00-template.md`        |

To add a new one: open the matching folder, click **Add file → Create new
file**, name it something like `elyndra-nightshade.md`, and paste in this
starting shape (this is called "front matter" — it goes at the very top of
every page):

```markdown
---
title: "Elyndra Nightshade"
status: "Alive"
summary: "A one-line hook — who they are, in a sentence."
tags: [npc, rogue]
---

Write the page here in normal Markdown.
```

The `status`, `summary`, and `tags` lines are optional — delete any you don't
need. The page will automatically show up in its section's list and in the
sidebar.

**Linking pages together:** just use a normal Markdown link with the other
page's URL, e.g. `[Elyndra](/characters/elyndra-nightshade/)` or
`[the Sunken Library](/locations/sunken-library/)`. The URL is always
`/section/filename-without-.md/`.

**Filenames become URLs**, so keep them lowercase with hyphens instead of
spaces (`sunken-library.md`, not `Sunken Library.md`).

## Optional: a PIN-based editor (no GitHub accounts needed)

By default, editing means people sign into GitHub. If you'd rather share a
single PIN and skip GitHub accounts entirely, this repo also includes a
`/edit/` page and a small backend for it in `cloudflare-worker/worker.js`.

**Why it needs a backend at all:** a PIN only means something if it's checked
somewhere the person editing can't see or tamper with — and actually writing
to your repo requires a GitHub token, which can never be placed in the
website's own code (anyone could open dev tools and read it, and then edit or
delete anything that token can touch). The Worker is a small program that
holds the token and PIN privately, checks the PIN on every request, and only
then talks to GitHub. This is a genuinely separate small project from the
site itself, so it takes a bit more setup than everything above — but it's
free and one-time.

**Setup:**

1. **Make a GitHub token scoped to only this repo.**
   Go to [github.com/settings/personal-access-tokens/new](https://github.com/settings/personal-access-tokens/new).
   Under "Repository access," choose "Only select repositories" and pick this
   one. Under "Permissions," give it **Contents: Read and write** and nothing
   else. Generate it and copy the token somewhere safe — you won't see it
   again.

2. **Create the Worker.**
   Go to [dash.cloudflare.com](https://dash.cloudflare.com), sign up free if
   you don't have an account, then **Workers & Pages → Create → Create
   Worker**. Give it any name, deploy the default, then click **Edit code**
   and replace everything with the contents of `cloudflare-worker/worker.js`
   from this repo. Click **Deploy**.

3. **Set the Worker's secrets.**
   In the Worker's **Settings → Variables and Secrets**, add these (mark
   `GITHUB_TOKEN` and `EDIT_PIN` as "Encrypt" / secret):
   | Name | Value |
   |---|---|
   | `EDIT_PIN` | your PIN, e.g. `4791` |
   | `GITHUB_TOKEN` | the token from step 1 |
   | `GITHUB_OWNER` | your GitHub username |
   | `GITHUB_REPO` | this repo's name |
   | `GITHUB_BRANCH` | `main` |
   | `ALLOWED_ORIGIN` | your site's URL, e.g. `https://you.github.io` |

   Redeploy after saving so the new variables take effect.

4. **Connect the site to the Worker.**
   Copy the Worker's URL (shown on its overview page, something like
   `https://your-worker-name.your-subdomain.workers.dev`). Paste it into
   `_config.yml` as `editor_worker_url`. Commit.

5. **Use it.** Go to `yoursite.com/edit/`, enter the PIN, and edit or create
   pages from a plain form — no GitHub account needed for whoever you give
   the PIN to.

**Be realistic about what a 4-digit PIN protects against:** it'll stop
randoms from stumbling in, but it's not resistant to someone deliberately
trying combinations. Don't reuse a PIN you use elsewhere, and if you ever
want it sturdier, change `EDIT_PIN` to a longer passphrase — the editor page
doesn't care how long it is.

GitHub's own pencil-icon editor (described above) still works for anyone with
repo access, side by side with this — they're independent.

## Deleting the example pages

Once you've got real content, delete the five `example-*.md` /
`session-00-template.md` files — they're just there to show the format.

## Search

The `/search/` page searches titles, tags, and page text — it needs no setup
and updates automatically as you add pages.
