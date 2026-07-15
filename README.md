# Midnight Cloud Check-in

GitHub Actions workflow that sends a daily relationship question via Telegram, even when the local machine is off.

This repo is public. All personal details (names, Telegram chat ID) are injected at runtime from **GitHub Secrets** — nothing identifying is stored in the code, README, or workflow logs.

## Setup (one-time)

### 1. Add repo secrets

In this repo → Settings → Secrets and variables → Actions → New repository secret:

| Name | Value |
|------|-------|
| `TELEGRAM_TOKEN` | Your bot token (from BotFather) |
| `ANTHROPIC_API_KEY` | Your key from console.anthropic.com |
| `CHAT_ID` | Your Telegram chat ID |
| `USER_NAME` | Your first name |
| `PARTNER_NAME` | Your partner's first name |

### 2. Enable Actions

Repo → Actions tab → Enable workflows (if prompted).

### 3. Test it

Actions tab → "Midnight Daily Check-in" → Run workflow → Run workflow.
You should get a Telegram message within 30 seconds.

## Schedule

Runs at **9:00 AM EDT** (UTC-4, summer). In winter (EST, UTC-5) it fires at 10 AM instead.
To lock it to 9 AM year-round, update the cron in the workflow to `0 14 * * *` Nov–Mar.

## How it works

1. Picks a question category by day of week (Mon=style, Tue=personality, etc.)
2. Calls Claude Haiku to craft a warm, specific question, using `USER_NAME`/`PARTNER_NAME` secrets
3. Sends it via Telegram to `CHAT_ID`
4. Waits up to 4 hours for a reply
5. Logs the Q&A as `[MIDNIGHT LOG]` in Telegram
6. A local agent reads those logs and saves them to memory
