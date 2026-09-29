# Discord Emoji Bot

Python Discord bot for managing application emojis and sending/reacting with them.

## Commands

- `/send name` — sends the emoji as a normal Discord message.
- `/emoji-add name file` — uploads an image as an application emoji. **Everyone can use this.**
- `/emoji-list [search]` — lists **24 emojis per page** in an **8-row × 3-column** grid.
- `/emoji-rename old_name new_name` — renames an application emoji. **Administrators only.**
- `/emoji-remove name` — removes an application emoji. **Moderators/admins only** (`Manage Messages`, `Manage Server`, or `Administrator`).
- **React theek** — message context-menu command. Right-click a message → Apps → **React theek** to add the `theek` emoji without copying a message ID.
- `$$name` while replying — reacts with the selected emoji; without a reply it falls back to `$name`.

The `/react` confirmation is ephemeral, so only the person who used the command sees it and can dismiss it. The actual reaction is placed on the target message normally, so other users can click it to add their own `theek` reaction.

## Permissions

`/emoji-add` intentionally has no moderator/admin restriction.

`/emoji-rename` is restricted to administrators both in Discord's command permissions and in the Python callback.

`/emoji-remove` is restricted to moderators/admins. In this bot, a moderator is a member with `Manage Messages` or `Manage Server`; administrators always qualify.

## Setup

1. Create a Discord application/bot.
2. Invite the bot with the required application-command permissions.
3. Copy `.env.example` to `.env`.
4. Set `TOKEN` (or `DISCORD_TOKEN`).
5. Optionally set `GUILD_ID` for fast development command registration.
6. Install dependencies:

```bash
python -m pip install -r requirements.txt
```

7. Run:

```bash
python bot.py
```

## Important

The bot needs permission to read message history and add reactions in channels where `/react` is used.

`database/emojis.json` stores the command-to-Discord-ID mapping. The actual application emoji asset is stored by Discord.

## Sticker formats
Discord custom stickers are not arbitrary files. This bot accepts PNG/APNG/GIF/JPG/JPEG/WebP as inputs, normalizes them to a 320x320 sticker canvas, and converts static JPG/JPEG/WebP to PNG. Animated inputs are normalized to GIF. Discord still enforces its 512 KiB sticker limit and server sticker-slot/permission limits.

## Message shortcuts
Enable **Message Content Intent** in the Discord Developer Portal. `$name` sends an emoji using the caller's webhook identity and deletes the command. `$$name` reacts to the message being replied to, or falls back to sending the emoji if there is no reply. `∆name` sends a sticker and deletes the shortcut.
