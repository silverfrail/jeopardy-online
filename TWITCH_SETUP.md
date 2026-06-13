# Setup Guide

This guide walks you through everything needed to run this Jeopardy game on **your own Twitch channel**. It is written for streamers with no technical background. Follow each step in order.

You will need:
- A free **GitHub** account
- A free **Firebase** account (Google account works)
- A free **Twitch** developer app registration
- About 30 minutes

---

## Step 1: Fork and Host the Project

You need your own hosted copy of these files. GitHub Pages is free and requires no server.

1. Go to https://github.com/silverfrail/jeopardy while signed into your GitHub account
2. Click **Fork** (top right) → **Create fork**
3. In your new forked repo, click **Settings** → **Pages** (left sidebar)
4. Under *Branch*, select `main` and click **Save**
5. After a minute, GitHub will show you a URL like:
   `https://YOUR-GITHUB-USERNAME.github.io/jeopardy/`
   
   Write this URL down — you will need it in Step 3.

---

## Step 2: Create a Firebase Project

Firebase is a free Google service that stores the game state in real time — the board, scores, and active clues all live here.

1. Go to https://console.firebase.google.com and sign in with your Google account
2. Click **Add project** → give it any name (e.g. `my-jeopardy`) → click through the setup wizard
3. Once the project is created, click **Realtime Database** in the left sidebar
4. Click **Create Database** → choose any region → start in **Test mode** → **Enable**
5. You will see a URL at the top of the database page that looks like:
   `https://my-jeopardy-default-rtdb.firebaseio.com/`
   
   Copy this URL — you will need it in Step 4.

6. Next, set your dashboard password. In the Firebase Realtime Database console, click the **+** button next to the root and add:
   - Key: `authValidation`
   - Expand it, add a child: Key: `secretKey`, Value: `choose-a-password`
   
   This is the password you will type to unlock the host dashboard.

---

## Step 3: Create a Twitch Developer App

This allows the game to read your chat and verify followers.

1. Go to https://dev.twitch.tv/console and log in with your Twitch account
2. Click **Register Your Application**
3. Fill in the form:
   - **Name:** anything (e.g. `My Jeopardy Game`)
   - **OAuth Redirect URL:** `https://YOUR-GITHUB-USERNAME.github.io/jeopardy/host_dashboard.html`
     *(replace YOUR-GITHUB-USERNAME with your actual GitHub username)*
   - **Category:** Game Integration
4. Click **Create**
5. On the next screen, click **Manage** next to your new app
6. Copy the **Client ID** shown on that page — you will need it in Step 4

---

## Step 4: Update the Code

Now you need to put your own values into the two game files. You can do this directly on GitHub — no coding tools required.

In your forked repo at `https://github.com/YOUR-GITHUB-USERNAME/jeopardy`, open **`host_dashboard.html`** and click the pencil (edit) icon.

Find these 4 lines near the top of the `<script>` section and replace them with your own values:

```js
const CLIENT_ID    = 'oetfyj917dbuh00b4ohrfimxv5ok6s';         // ← your Twitch Client ID from Step 3
const REDIRECT_URI = 'https://silverfrail.github.io/jeopardy/host_dashboard.html';  // ← your GitHub Pages URL + /host_dashboard.html
const BROADCASTER  = 'silverfrailttv';                           // ← your Twitch username (lowercase)
```

And a few lines below, find:

```js
databaseURL: 'https://jeopardy-stream-default-rtdb.firebaseio.com'  // ← your Firebase URL from Step 2
```

Replace all four values with yours, then click **Commit changes**.

Now open **`jeopardy_preview.html`** and do the same for the Firebase URL:

```js
databaseURL: 'https://jeopardy-stream-default-rtdb.firebaseio.com'  // ← your Firebase URL from Step 2
```

Commit that change too.

---

## Step 5: Add OBS Browser Sources

The game uses two browser windows — one for you (the host) and one that goes on stream.

**Host Dashboard** (open this in your regular browser, not on stream):
```
https://YOUR-GITHUB-USERNAME.github.io/jeopardy/host_dashboard.html
```

**Stream Overlay** (add this as a Browser Source in OBS):
```
https://YOUR-GITHUB-USERNAME.github.io/jeopardy/jeopardy_preview.html
```

Recommended OBS Browser Source settings:
- Width: `1920`, Height: `1080`
- Check **Shutdown source when not visible**

---

## Step 6: Connect Your Twitch Account in the Dashboard

1. Open your host dashboard URL in your browser
2. Enter the password you set in Step 2
3. In the **Twitch** panel on the right, click **Connect Twitch Account**
4. Authorize the app on Twitch
5. The Twitch status dot in the header will turn green — you're connected

---

## Step 7: Load a Game Board and Play

1. In the **Game Management** panel, click **Gen Prompt** to generate a prompt for ChatGPT
2. Paste the prompt into ChatGPT, copy the JSON it returns
3. Paste the JSON into the text box in Game Management
4. Click **Upload Board**
5. The board appears in OBS — click any dollar value tile to reveal a clue
6. Viewers type `!guess [their answer]` in chat — their guesses appear in your dashboard for you to judge

---

## Follower-Only Guessing

There is a **Followers only** toggle in the Twitch panel.

| Setting | Behavior |
|---|---|
| **On (default)** | Only viewers who follow your channel can submit guesses. Requires Twitch connection. Non-followers get a message in chat telling them to follow first. |
| **Off** | Anyone in chat can guess. Twitch connection is not required. |

---

## Token Expiry

The Twitch connection uses a token that **expires after approximately 4 hours**. If the Twitch dot turns gray mid-stream, click **Connect Twitch Account** again to refresh it. Nothing else in the game is affected.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| Dashboard won't unlock | Check that `authValidation/secretKey` is set correctly in your Firebase console |
| Board doesn't appear in OBS | Make sure both files point to your Firebase URL (Step 4) |
| Follower check not working | Make sure you completed the Twitch connection (Step 6) and that your Twitch OAuth Redirect URL exactly matches your GitHub Pages URL |
| Chat replies not sending | Your Twitch app OAuth Redirect URL must exactly match the URL in the code — even a trailing slash difference will break it |
