# WC26 Predictor League — self-hosted setup

One HTML file, one free Firebase database. About 10 minutes, no coding.

## 1. Create the free database (Firebase)

1. Go to **console.firebase.google.com** → **Add project** → name it (e.g. `wc26-league`) → turn **off** Google Analytics → Create.
2. In the left menu: **Build → Firestore Database → Create database** → location **europe-west2 (London)** → start in **production mode** → Create.
3. Open the **Rules** tab, replace everything with the below, then press **Publish**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /kv/{doc} {
      allow read, write: if true;
    }
  }
}
```

(This opens one collection called `kv` to anyone with the link — fine for a friends league, don't reuse this project for anything else.)

4. Click the **gear icon → Project settings → General**. Scroll to **Your apps**, click the **`</>` (Web)** icon, register the app (no hosting needed). Firebase shows you a config block — you only need two values from it: **apiKey** and **projectId**.

## 2. Put your keys in the file

Open `index.html` in any text editor and find this near the top of the `<script>`:

```js
const FIREBASE = {
  apiKey:    'AIzaSyBTSlVrSd_w97EKRUGfuNVZ5IoZlyzVyAY',
  projectId: 'wc-2026-score-predictor'
};
```

Paste your two values in. Save. (The apiKey is not a secret in Firebase's model — it identifies the project; the rules above are what control access.)

## 3. Publish on GitHub Pages

1. Commit `index.html` (and this README) to the root of your repository.
2. Repo **Settings → Pages → Source: Deploy from a branch → main / root → Save**.
3. After a minute your league is live at `https://<your-username>.github.io/<repo-name>/`.

Send that link to everyone.

## 4. First thing YOU do

Open the site, go to the **Admin** tab and **set the PIN before anyone else does** — the first PIN set claims admin.

## Running the league

- Friends open the link, name their board, score all 72 group games, build their knockout bracket, and submit once. No edits after submitting. Picks lock at the opening kickoff (Admin can allow late boards — they get a "Late" badge).
- **At the end of each match day:** Admin tab → type the day's scores (4–6 games, ~30 seconds) → Save. Everyone's points and the league table update automatically. Others press "⟳ Reload latest" on the Results tab to pull the newest data.
- Scoring: **5** exact score, **3** correct result, **0** otherwise. Knockouts score 3 if the team backed in that bracket slot wins it; 5 only for both teams + exact score.

## Notes

- Each person's in-progress picks save in their own browser until they submit, so don't switch devices mid-draft.
- One board per person is enforced per browser, not cryptographically — it's an honour system.
- Knockout draws: enter the 90-minute (or after-extra-time) score plus the team that went through as the winner.
