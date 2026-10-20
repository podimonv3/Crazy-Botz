# Koyeb deployment

This project uses a Docker Web Service. The health endpoint responds `OK` at `/`, `/health`, and `/healthz` on Koyeb's `PORT` (default `8080`).

## Required environment variables

- `APP_ID`: numeric Telegram API ID
- `API_HASH`: Telegram API hash
- `BOT_TOKEN`: bot token from BotFather
- `DB_URI`: MongoDB connection URI
- `USER_SESSION`: Pyrogram user session string (this project starts a user client too)
- `DB_NAME`: optional database name; defaults to `Adv_Auto_Filter`

## Deploy

1. Upload the repository files to a new GitHub repository.
2. In Koyeb, create a Web Service from that GitHub repository.
3. Select Dockerfile build, then add the environment variables above as secrets/environment variables.
4. Expose the service on the configured `PORT` (normally 8080) and deploy.
5. Check `/health` on the generated service URL and review logs for Telegram/MongoDB startup errors.

Never commit tokens, API hashes, MongoDB URIs, or session strings. The app does not intentionally delete existing MongoDB collections. Index creation may add indexes to `Filter_Collection`; ensure the database user has index-creation permissions.

## Database/search notes

- Search indexes are created once per process, not on every query.
- Search results are filtered by group and active channel IDs in MongoDB before being fetched.
- The existing text-index search model and configurable `accuracy` threshold are preserved. MongoDB text-score thresholds are data-dependent, so tune `accuracy` against real titles if results are too strict or broad.
