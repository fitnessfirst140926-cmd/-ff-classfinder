# Fitness First Class Finder — auto-updating

Pulls the live (public, no-login-required) Fitness First SG timetable
every week and rebuilds the Class Finder app automatically, hosted
free on GitHub Pages.

## One-time setup (~5 minutes)

1. **Create a new GitHub repo** (public or private both work) and
   upload everything in this folder to it, keeping the same structure:
   ```
   scrape_timetable.py
   build_app.py
   .github/workflows/update-timetable.yml
   ```

2. **Run it once manually** to generate the first version:
   - Go to the repo's **Actions** tab
   - Click **"Update Fitness First timetable"** → **Run workflow**
   - Wait ~30 seconds for it to finish (it'll commit `classes.json`,
     `classes_min.json`, and `docs/index.html`)

3. **Enable GitHub Pages**:
   - Repo **Settings** → **Pages**
   - Source: **Deploy from a branch**
   - Branch: **main**, folder: **/docs**
   - Save

4. Your app is now live at:
   `https://<your-username>.github.io/<repo-name>/`

That's it. Every Monday at 11am Singapore time, GitHub automatically
re-runs the scraper, rebuilds the app, and publishes the update — with
zero manual steps from you going forward.

## Changing the schedule

Edit the `cron` line in `.github/workflows/update-timetable.yml`.
Cron is in UTC — Singapore is UTC+8. For example, `0 22 * * 0` runs
every Sunday at 6am Singapore time (10pm UTC Sunday).

## Running it locally (optional)

```
pip install requests
python3 scrape_timetable.py   # writes classes.json / classes_min.json
python3 build_app.py          # writes docs/index.html
```

## Notes

- The Fitness First timetable endpoint returns a **rolling 7-day
  window starting today**, not a fixed Sun–Sat week — so "Monday" in
  the app always means the next upcoming Monday, refreshed weekly.
- The endpoint is public (confirmed: no cookies/session required to
  view it — only booking a class needs login), so nothing here uses
  or stores any account credentials.
- If Fitness First changes their API shape, `scrape_timetable.py` is
  the only file that should need updating.
