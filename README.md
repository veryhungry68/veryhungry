# GitHub → Telegram Random Photo Publisher

## Repository layout

```text
.
├── publisher.ipynb
├── comments.txt
├── state.json
├── images/
└── .github/workflows/publish.yml
```

## Setup

1. Create a **public** GitHub repository.
2. Upload these starter files and folders.
3. Add JPG/JPEG/PNG/WEBP images to `images/` (maximum 10 MB each).
4. Edit `comments.txt`: one caption per line.
5. In Telegram, create a bot with @BotFather.
6. Add the bot to your Telegram channel as an administrator. Give it permission to post and delete messages.
7. In GitHub, open **Settings → Secrets and variables → Actions → New repository secret**.
   - `TELEGRAM_BOT_TOKEN`: the token from @BotFather.
   - `TELEGRAM_CHAT_ID`: `@your_channel_username` or the channel's numeric chat ID.
8. In GitHub, open **Settings → Actions → General → Workflow permissions** and allow read and write permissions if repository policy requires it. The workflow also declares `contents: write`.
9. Ensure the workflow file is on the repository's default branch. Open **Actions** and enable workflows if prompted.
10. Use **Actions → Scheduled Telegram Photo Publisher → Run workflow** to test immediately.

## Schedule

The included schedule runs every 30 minutes at minutes 17 and 47 in `America/Los_Angeles` time. Edit the cron expression in `.github/workflows/publish.yml` to change it. GitHub scheduled workflows are best-effort and can be delayed.

## How it works

The notebook lists image files through GitHub's public repository Contents API, downloads one, chooses a random caption, uploads it to Telegram, then attempts to delete the previously recorded bot post. It writes the latest message ID and image name to `state.json`. GitHub Actions commits that state file so the next run can continue.

## Important limitations

- The bot must be an administrator of the channel with permission to post and delete messages.
- Telegram only permits deletion of messages within 48 hours; channel deletion permissions are required.
- A photo uploaded to Telegram via multipart upload must be no larger than 10 MB.
- The repository is public, so all images, captions, notebook code, and state metadata are public. Do not put private material or credentials in it.
- The workflow stores the bot token only in GitHub Actions Secrets.
- Scheduled workflows run on GitHub-hosted cloud runners. Standard hosted runner usage is free for public repositories.
- GitHub may delay scheduled runs during high load. In public repositories, scheduled workflows can be disabled after 60 days without repository activity.
- This starter uses a single `state.json` record and is intended for one publishing schedule/channel. Do not run overlapping copies against the same channel.
