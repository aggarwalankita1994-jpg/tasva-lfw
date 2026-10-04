# Tasva × LFW 2026: Show Desk

The live tracker for models and influencers for the Tasva show at Lakmé Fashion Week.

| Who | How they get in | What they can do |
|---|---|---|
| **Ankita** | Sign in with Google | Everything: edit, first approval, Settings, Excel sync, delete |
| **Piyush** | Sign in with Google | Edit models and influencers, final approval |
| **Anyone else with the link** | Just open it | View only |

The data lives in Firebase (Google's database service), not in this repository.

---

## One-time setup (about 15 minutes)

### 1. Put the app on GitHub
1. Sign in at github.com, then **+ → New repository**. Name it `tasva-lfw` and set it to **Public**. Free GitHub Pages needs a public repository. That's fine, because this code contains no data.
2. On the new repository page, click **uploading an existing file**. Drag in everything from the `tasva-show-desk` folder: `index.html`, `firebase-config.js`, `firestore.rules`, `README.md` and the `photos` folder.
   **Don't upload `tasva-backup-for-restore.json`.** It stays on your laptop.
3. Click **Commit changes**.
4. Go to **Settings → Pages**. Under "Build and deployment", choose **Deploy from a branch**, set the branch to **main** and the folder to **/ (root)**, then **Save**.
5. After about a minute, the link appears at the top: `https://<your-username>.github.io/tasva-lfw/`. It will say "Almost there" until step 3 below is done.

### 2. Create the Firebase database
1. Go to console.firebase.google.com, then **Create a project** (e.g. `tasva-lfw`). You can turn Google Analytics off.
2. **Build → Firestore Database → Create database.** Choose **production mode** and the location **asia-south1 (Mumbai)**.
3. **Build → Authentication → Get started → Google → Enable.** Pick your support email, then **Save**.
4. **Authentication → Settings → Authorized domains → Add domain:** `<your-username>.github.io`.
5. **Firestore Database → Rules:** delete what's there and paste the whole of `firestore.rules`. In the line
   `function ankitaEmail() { return "REPLACE_WITH_ANKITA_EMAIL"; }`
   put **your Google email in lowercase**, then **Publish**. Make this edit only in the Firebase console, not in the GitHub copy, so your email doesn't become public.

### 3. Connect the app to Firebase
1. In Firebase, go to **⚙ Project settings → General → Your apps → Web (`</>`)**. Register an app called `show-desk` (no hosting needed).
2. Copy the values in `firebaseConfig` (apiKey, authDomain, projectId, storageBucket, messagingSenderId, appId).
3. In GitHub, open `firebase-config.js` and click the ✏️ pencil. Replace each `PASTE_…` value with yours, then **Commit changes**.
   These values are meant to be public. The rules from step 2 are what protect the data.
4. Wait a minute and reload the app link.

### 4. Bring your data over and give Piyush access
1. Open the app link and click **Sign in**, using the Google account whose email you put in the rules. Your name chip shows **Ankita**.
2. Go to **Settings → Restore from backup**, choose `tasva-backup-for-restore.json` (from the LFW folder on your laptop) and confirm. All 98 models, their history and the Excel sync status come across.
3. Still in **Settings → People & access**, enter **Piyush's Google email** and press **Save access**.
4. Send Piyush the link. He clicks **Sign in**, and his chip shows **Piyush**.
   If Piyush doesn't use a Google account: in Firebase, go to **Authentication → Sign-in method → Add new provider → Email/Password**, then **Users → Add user** with his email and a password. Tell Claude, and the app can show an email-and-password sign-in as well.

---

## Day to day
- **Open the link on any phone or laptop.** On phones, the tabs move to the bottom of the screen and models show as cards. Tap a card for the full record.
- **Approvals:** your Approve sends a model to "Approval pending Piyush". Only Piyush can approve from there, and the database enforces this even if someone tampers with the page.
- **Excel sync** (Ankita only): the same as before. "Update app from Excel" reads your master. "Download updated Excel copy" makes a new file, and the master is never changed.

## If something looks wrong
- **"Almost there":** `firebase-config.js` still has `PASTE_…` values (step 3).
- **Sign-in popup closes straight away:** check the authorized domain (step 2.4).
- **"Not allowed for your role":** the rules aren't published, or the email in them doesn't match your sign-in email exactly.
- **You're signed in but see "View only":** your email isn't the one in the rules (Ankita), or isn't saved in Settings → People & access (Piyush).
