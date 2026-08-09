# Feedback Loom

A single-page store feedback portal with a customer view, a CRO-assisted
view, and an admin dashboard with charts and CSV export.

- **Frontend:** plain HTML/CSS/JS, no build step
- **Database:** Firebase Firestore
- **Hosting:** designed to deploy as a static site (Vercel, Firebase Hosting,
  Netlify, GitHub Pages — any of them work)

## Structure

```
index.html   the entire app (markup, styles, and logic in one file)
.gitignore
```

## Setup

1. Create a Firebase project at https://console.firebase.google.com
2. Enable **Firestore Database** (Build > Firestore Database > Create database)
3. Register a **Web app** (Project settings > General > Your apps > `</>`)
   and copy the `firebaseConfig` object it gives you
4. Open `index.html`, find the `firebaseConfig` object near the top of the
   `<script>` tag, and replace it with your own values
5. In Firestore's **Rules** tab, set:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /entries/{entryId} {
         allow create: if true;
         allow read, update, delete: if false;
       }
       match /config/{docId} {
         allow read, write: if true;
       }
     }
   }
   ```

## Deploy on Vercel

1. Push this folder to a GitHub repo
2. In Vercel: Add New > Project > Import this repo
3. No build command or output directory needed — it's a static site
4. Deploy

If your Firebase API key has HTTP referrer restrictions (Google Cloud
Console > APIs & Services > Credentials), add your Vercel domain
(`*.vercel.app/*`, and any custom domain) to the allowed list.

## Views

The app is a single file that switches views based on the `?view=` query
param:

- `?view=customer` — self-scan feedback form
- `?view=cro` — CRO-assisted feedback form
- `?view=admin` — password-gated dashboard (default password is set on
  first load; change it from the dashboard's settings panel)
- no param — home page with QR codes linking to the two form views
