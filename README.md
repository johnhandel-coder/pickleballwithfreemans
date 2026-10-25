# 🥒 Pickleball Tracker

A one-page, phone-friendly site for tracking our four-person doubles record.

- Tap the winning team, tap **Pickled them!** if it was 11-0, tap **Save**.
- Each winner gets **1 point**, or **2 points** for a pickle. Losers get 0.
- **Playing on** date at the top of Log Game: defaults to today, or pick an earlier date to enter a past session.
- **Standings**: points, W-L, win %, pickles given/taken, for Today, This month, All time, or any month or day played.
- **Streaks**: 🔥 W2+ win streaks and 🧊 L3+ losing streaks next to each name.
- **Records**: longest win streak, most pickles, got pickled most, best duo.
- **Matchups**: head-to-head record for each of the three team splits.
- **Partner stats**: each player's record and points with each partner (💪 best, 😬 worst).
- **History**: every game grouped by day. Tap a game to edit or delete it.
- **⚙️ Settings**: rename players, export/import a JSON backup.

Out of the box the site runs in **local mode**, which saves scores only in the browser you're using. To share one live scoreboard across all four phones, set up Firebase (free) below.

---

## 1. Set up Firebase (one time, ~10 minutes)

1. Go to <https://console.firebase.google.com> and click **Create a project** (e.g. `pickleball-tracker`). You can turn Google Analytics off.
2. In the project, click the **Web** icon (`</>`) to add a web app. Give it any nickname. You don't need Firebase Hosting.
3. Firebase shows a `firebaseConfig` object. Copy its values into the `FIREBASE_CONFIG` block near the top of the `<script>` in `index.html`:

   ```js
   const FIREBASE_CONFIG = {
     apiKey: "AIza...",
     authDomain: "pickleball-tracker.firebaseapp.com",
     projectId: "pickleball-tracker",
     storageBucket: "pickleball-tracker.appspot.com",
     messagingSenderId: "1234567890",
     appId: "1:1234567890:web:abc123"
   };
   ```

   These values aren't secret. Firebase web configs are meant to be public.
4. In the left menu, go to **Build → Firestore Database → Create database**. Pick a location near you and choose **Start in production mode**.
5. Open the **Rules** tab, replace the contents with the rules below, and click **Publish**:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /games/{id}   { allow read, write: if true; }
       match /meta/{id}    { allow read, write: if true; }
     }
   }
   ```

   These rules let anyone who has the site's URL read and edit scores, so keep the link within the group.

Reload the page. The yellow "Local mode" banner should be gone. Log a game on one phone and it appears on the others.

> Already logged games in local mode? Before adding the config, open ⚙️ → **Export JSON** on that phone. After switching to Firebase, use ⚙️ → **Import JSON** to bring the games over.

## 2. Host it on GitHub Pages

1. Create a new repository on GitHub (e.g. `pickleball`). It can be public or private, but Pages on private repos needs a paid plan.
2. Upload `index.html` (and this README) to the repo, or push it with git:

   ```bash
   git init
   git add index.html README.md
   git commit -m "Pickleball tracker"
   git branch -M main
   git remote add origin https://github.com/<you>/pickleball.git
   git push -u origin main
   ```
3. In the repo, go to **Settings → Pages**. Under **Build and deployment**, choose **Deploy from a branch**, select **main** and **/ (root)**, then click **Save**.
4. After a minute the site is live at `https://<you>.github.io/pickleball/`.

## 3. Put it on everyone's home screen

- **iPhone (Safari):** Share → **Add to Home Screen**
- **Android (Chrome):** ⋮ menu → **Add to Home screen**

It then opens full-screen like an app.

## Notes

- With Firebase, games logged with weak court Wi-Fi are saved on the phone and sync automatically when the connection comes back.
- Players are stored by slot (1–4), so renaming someone in ⚙️ Settings keeps all their history.