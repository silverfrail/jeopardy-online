# Twitch Setup Guide

## The "Connect Twitch Account" Button

When you open the host dashboard, you'll see a **Connect Twitch Account** button in the Twitch panel on the right side. Here's what it does and when you need it.

---

## What It Does

Clicking this button links the dashboard to your Twitch account. This allows the game to automatically check whether someone guessing in chat is a **follower of your channel** before accepting their answer.

When a viewer types `!guess [answer]` in chat:
1. The dashboard checks if that viewer follows your channel
2. If they do — their guess is accepted
3. If they don't — their guess is silently ignored

---

## Do You Actually Need It?

**Only if you want follower-only guessing.** There is a toggle in the Twitch panel called **Followers only**. 

- **Checked (default):** Only followers can guess. Requires the Twitch connection.
- **Unchecked:** Anyone in chat can guess, regardless of whether they follow. The Twitch connection is not needed in this case.

If you uncheck **Followers only**, you can skip connecting your Twitch account entirely and the game will work just fine.

---

## Token Expiry

The Twitch connection uses a short-lived access token that **expires after approximately 4 hours**. If you're running a long stream, you may need to click **Connect Twitch Account** again to refresh it. You'll know it's expired when the Twitch status dot in the header turns gray.

If this happens mid-game, simply click **Connect Twitch Account**, authorize again, and guessing will resume normally. Nothing else in the game is affected.

---

## Summary

| Situation | Do you need to connect Twitch? |
|---|---|
| Followers-only guessing (default) | ✅ Yes |
| Open guessing (anyone can play) | ❌ No — just uncheck "Followers only" |
