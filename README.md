# Ledger — a private budget tracker

A single-page budget tracker with email/password login, backed by Firebase.
Everyone who signs in shares the same ledger (good for a couple or a small
household). Only people you personally add as Firebase users can log in —
there's no sign-up form, so access is limited by design.

## 1. Create a Firebase project

1. Go to [console.firebase.google.com](https://console.firebase.google.com) → **Add project**.
2. Once it's created, click the **`</>`** (web) icon to register a web app. You don't need Firebase Hosting for this — GitHub Pages will serve the file.
3. Copy the `firebaseConfig` object it gives you.

## 2. Turn on email/password login

1. In the console: **Build → Authentication → Get started**.
2. Under **Sign-in method**, enable **Email/Password**.
3. Under the **Users** tab, click **Add user** for each person you want to have access — just an email and a password. This is the only way accounts get created, since the app has no sign-up form.

## 3. Create the database

1. **Build → Firestore Database → Create database**. Start in production mode.
2. Go to the **Rules** tab and replace the contents with what's in `firestore.rules` in this project, then click **Publish**.
   This locks every read/write to signed-in users only.

## 4. Connect the app to your project

1. Open `index.html`.
2. Find the `firebaseConfig` object near the bottom of the file and replace the placeholder values with the ones you copied in step 1.

```js
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

## 5. Put it on GitHub Pages

1. Create a new GitHub repository and push these files (`index.html`, `firestore.rules`, `README.md`) to it.
2. In the repo: **Settings → Pages**.
3. Under **Source**, choose **Deploy from a branch**, pick your default branch (e.g. `main`) and the `/ (root)` folder, then **Save**.
4. GitHub will give you a URL like `https://yourusername.github.io/your-repo/` — that's your live tracker.

## 6. Authorize the domain in Firebase

Firebase blocks logins from domains it doesn't know about:

1. **Authentication → Settings → Authorized domains → Add domain**.
2. Add your GitHub Pages domain, e.g. `yourusername.github.io`.

## How access control works

- There's no sign-up screen in the app — the only accounts that exist are the ones you manually add in **Authentication → Users**.
- Firestore rules (`firestore.rules`) reject any request that isn't from a signed-in user, so even someone who found the URL can't read or write data without an account.
- To remove someone's access later, just delete their user in the Firebase console.

## What's in the app

- **Add entries** — income or expense, with amount, category and an optional note.
- **Live totals** — balance, income and expenses update instantly for everyone signed in, since data is shared in real time through Firestore.
- **Delete entries** — any signed-in user can remove an entry (it's a shared ledger).

## Customizing

- To make budgets *private per person* instead of shared, store transactions under `users/{uid}/transactions` instead of a flat `transactions` collection, and update the rules and queries to match — happy to help with that change if you'd rather go that route.
- The look is plain HTML/CSS/JS (no build step), so you can restyle it directly in `index.html`.
