# Sangamam26 badge maker

A static web app. Members pick **I'm attending** or **I'm volunteering**, enter name, email and photo,
and get the badge with their photo (background removed, white) to download, share or email to themselves.
The photo is processed in the browser. There is no server to maintain.

## Files
- `index.html`: the whole app
- `templates/attending.png`, `templates/volunteering.png`: your badge templates
- `apps-script/Code.gs`: optional Google Sheet log + email (Part 2)

## Part 1: publish on GitHub Pages (about 5 minutes)
1. Create a GitHub account or organisation for the chapter, then **New repository** (public), e.g. `sangamam26-badge`.
2. **Add file > Upload files**, drag in `index.html` and the `templates` folder, **Commit**.
3. **Settings > Pages > Build and deployment**: Source = *Deploy from a branch*, Branch = `main`, folder `/ (root)`, **Save**.
4. After a minute the app is live at `https://<your-username>.github.io/sangamam26-badge/`.

That is enough for a working app: members can download or share the badge.

## Part 2: record names/emails in a Google Sheet and email the badge (optional, free)
1. Create a new Google Sheet, e.g. "Sangamam26 badge log". (Optional: the script creates its own sheet if you skip this.)
2. In the Sheet: **Extensions > Apps Script**. Delete the sample code, paste `apps-script/Code.gs`, **Save**.
3. In the editor pick the function **setup** and press **Run**. Approve the permissions (Sheets and Gmail send). This also creates the "Badges" tab.
4. **Deploy > New deployment > Web app**. Execute as: **Me**. Who has access: **Anyone** (not "Anyone with Google account"). **Deploy**.
5. Copy the **Web app URL** (it ends in `/exec`) and paste it into `index.html` at `APPS_SCRIPT_URL: ""`, then commit to GitHub.
6. Test: open the web app URL in a browser. You should see `{"ok":true,"service":"Sangamam26 badge log",...}`.

Every badge request adds a row to the **Badges** tab (time, name, email, badge, action, email status).
Problems are written to an **Errors** tab.

**If you change Code.gs later**, you must publish it again: **Deploy > Manage deployments > pencil icon > Version: New version > Deploy**.
Saving alone does not update the live web app.

Email limits: about 100 emails/day on a free Gmail account, about 1,500/day on Google Workspace.
Each address can receive at most 5 badge emails (change `MAX_EMAILS_PER_ADDRESS`).

## Replacing the templates
Keep the same file names. Higher-resolution templates (e.g. 2000 x 2000 px) give a sharper badge.
If the photo circle sits in a different place, adjust that template's entry in `CIRCLE` near the top of the script in `index.html`
(`cx`, `cy` = centre as a fraction of width, `r` = photo radius, `ring` = white ring radius).

## Notes
- Background removal uses the open-source `@imgly/background-removal` library, loaded from a CDN in the browser.
  The first run downloads an AI model (tens of MB) which the browser then caches.
- The library is AGPL-licensed. Keeping this repository public satisfies that for this use.

## Branding
The header shows the PMI Chennai logo (`assets/pmi-chennai-logo.png`), the app owner line
("Arumugam Mariappan, VP Technology, PMI Chennai Chapter") and "Made for PMI Chennai Chapter, Sangamam26".
Colours are CSS variables at the top of `index.html`. Default share hashtags are in `CONFIG.HASHTAGS`.
Photo-circle positions are set per template in `CONFIG.CIRCLE` (the two templates differ slightly).
