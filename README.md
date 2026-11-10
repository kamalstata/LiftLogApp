# LiftLog – Gym tracker PWA

A daily dashboard (water, supplements, habits, streaks, level/XP), a workout log with rest timer and previous-session comparison, and animated technique guides.
Each user signs in with **their own Google account**. Their history is saved to a Google Sheet called
**"LiftLog Data"** in **their own Google Drive**. You, the app owner, never see anyone's data.

## Files

| File | What it is |
|---|---|
| `index.html` | The whole app |
| `config.js` | **The only file you edit.** Paste your Google Client ID here |
| `manifest.json`, `icons/` | Make it installable as an app on Android and iPhone |
| `sw.js` | Lets the app open without internet (at the gym) |

---

## Step 1 – Put it on GitHub Pages (about 5 min)

1. Sign in at https://github.com and click **New repository**. Name it `liftlog` and make it **Public**.
2. Click **uploading an existing file** and drag in **all** the files and the `icons` folder. Then click **Commit changes**.
3. Go to **Settings → Pages**. Under *Branch*, choose `main` and `/ (root)`, then click **Save**.
4. After 1–2 minutes your app is live at `https://YOUR-USERNAME.github.io/liftlog/`.

The app already works at this point, saving on the phone only. Step 2 adds Google sign-in.

## Step 2 – Turn on Google sign-in (about 10 min, free)

1. Go to https://console.cloud.google.com and create a new project called **LiftLog**.
2. Go to **APIs & Services → Library** and enable **Google Sheets API** and **Google Drive API**.
3. Open **Google Auth Platform** (older screens call it the *OAuth consent screen*):
   - **Branding:** app name `LiftLog`, your support email, and your developer email.
   - **Audience:** *External*.
   - **Data access → Add or remove scopes:** add these two only:
     - `.../auth/drive.file`. This lets LiftLog see **only the sheet it creates**, not the rest of the user's Drive.
     - `.../auth/userinfo.email`
4. **Clients → Create client:**
   - Application type: **Web application**.
   - **Authorised JavaScript origins:** `https://YOUR-USERNAME.github.io`. Use the domain only, with no `/liftlog` and no trailing slash.
   - Click Create and copy the **Client ID**. It ends with `.apps.googleusercontent.com`.
5. On GitHub, open `config.js`, click the ✏️ pencil, paste the Client ID in place of `PASTE_YOUR_CLIENT_ID...`, and commit.

### Who can sign in?
- While the app is in **Testing** (under Audience), only the Google accounts you add as **Test users** can sign in. You can add up to 100. This is good for you and friends.
- To let anyone sign in, click **Publish app** under Audience. LiftLog only uses non-sensitive permissions, so Google does not require a security review. Users may still see an "unverified app" warning until you complete Google's optional brand verification.

## Step 3 – Install on the phone

- **Android (Chrome):** open the link, then tap ⋮ → **Install app** or **Add to Home screen**.
- **iPhone (Safari):** open the link, then tap Share → **Add to Home Screen**.

## How saving works

1. Everything is saved on the phone first, so the app works with no internet.
2. When you finish a workout, log water, tick a supplement or habit, or change a plan or setting, it syncs to the sheet if you're signed in.
3. Google sign-in lasts about 1 hour. After that the top-right shows **"Tap to sync"**. One tap re-connects and uploads, usually with no password needed.
4. Sign in with the same Google account on another phone and your history appears there too.
5. Users can open their sheet from **Plans → Google account → Open my Google Sheet**. It has these tabs:
   - **Workouts:** one row per session: date, plan, duration, volume
   - **Sets:** one row per completed set: exercise, kg, reps, volume. Use this tab for pivot tables and charts.
   - **Plans:** the user's routines
   - **Daily:** one row per day: water (ml), whether the goal was met, supplements taken, habits done
   - **Settings:** used by the app; don't edit.

Treat the sheet as **read-only**. Make changes in the app, because the app rewrites the sheet on every sync.

## Updating the app later

Upload the changed files. Then open `sw.js` and change `liftlog-v1` to `liftlog-v2` (and so on), so phones pick up the new version.
