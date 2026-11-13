# Teams Transcripts

**Ask Claude what was said in your Teams meetings. It downloads meeting transcripts with speaker names, meeting chat, participants and recordings, and exports Teams chats and channels, from the Teams desktop app or Teams for the web on your Mac.**

Version **1.22.0** · macOS · Claude Desktop extension (.mcpb) · Teams desktop app or Teams for the web

- [Install: for people](#install-for-people)
- [Install: for AI agents](#install-for-ai-agents)
- [What it can and can't do](#what-it-can-and-cant-do)
- [What you need](#what-you-need)
- [What you can ask](#what-you-can-ask)
- [Available tools](#available-tools)
- [Settings](#settings)
- [Security and privacy](#security-and-privacy)
- [Troubleshooting](#troubleshooting)
- [Update and remove](#update-and-remove)

---

## Install: for people

You need a Mac with Claude Desktop 2.7032.0 or later, and the Microsoft Teams desktop app or Chrome or Edge for
Teams for the web. Open Terminal and run:

```bash
curl -fsSL https://wpp-jamie-mills.github.io/teams-transcripts-site/install.sh | bash
```

The installer downloads the latest extension, checks its SHA-256 before anything changes, opens Claude Desktop's
install dialog, and waits until the extension is switched on and running. You don't need Python, Node, git, a Google
Cloud project or a configuration file.

Then:

1. Click **Install** in the dialog Claude Desktop opens. You can leave its
   settings blank: it reads the Teams desktop app unless you switch on
   **Default to Teams for the web**. If Claude Desktop shows the extension
   switched off, switch it on: Settings > Extensions > Teams Transcripts.
2. Sign in to the Teams desktop app with every work account you want to read.
   For Teams for the web there is nothing to open: the extension starts its
   own Chrome or Edge window when it first needs one.
3. Start a new chat (chats opened before the extension was on don't see its
   tools) and ask "Is Teams ready?". If Teams needs you to sign in, Claude
   says so and brings the window to the front: sign in there (MFA included)
   and Claude carries on. Then ask, for example, "List my meetings with
   transcripts from the last week".

After that it keeps itself up to date: once a day it checks for a new version, and Claude offers it on an update card
in the chat. Click Update now, then Install in Claude Desktop's dialog; nothing is installed until you do. Running the
same command again also updates. Your settings are kept.

## Install: for AI agents

Use these steps if you are an AI agent installing this for a user.

1. **Check the machine.** Continue only if `uname -s` prints `Darwin` and `/Applications/Claude.app` exists. Otherwise tell the user it needs a Mac with Claude Desktop 2.7032.0 or later.
2. **Run the installer** with this one command. It downloads the extension, checks its SHA-256 (nothing changes before the check passes) and opens Claude Desktop's install dialog. With no terminal it never prompts, and `--no-wait` makes it exit once the dialog is open. It exits non-zero, with the reason on stderr, if anything fails.

```bash
curl -fsSL https://wpp-jamie-mills.github.io/teams-transcripts-site/install.sh | TEAMS_TRANSCRIPTS_INSTALL_TTY=/dev/null bash -s -- --no-wait
```

3. **Hand over to the user.** Ask them to click Install in the dialog Claude Desktop opened (they can leave its settings blank), and to switch the extension on if Claude Desktop shows it off (Settings > Extensions). You can't do these for them.
4. **Verify.** Run the installer again with `--status` (after `bash -s --`). It should show `Ready: yes`; otherwise its `Next:` line says the one thing left to do. Then ask the user to start a new chat (chats opened while the extension was off don't see its tools) and ask "Is Teams ready?". If Teams needs a sign-in, only the user can sign in; never ask for their password.
5. **Updates look after themselves.** Once a day the extension checks for a new version and Claude shows the user an update card. Only the user can click Update now and then Install; to update straight away, run the install command again.

## What it can and can't do

- **Can:** list meetings with transcripts and recordings in every signed-in
  work account and tenant; download a meeting's transcript (as caption cues,
  speaker turns or plain text) with speaker names, its chat with reactions,
  and who organised it, was invited, spoke and joined; save a recording you
  own; capture a recording's presentation slides and build a notes-page deck;
  export ordinary chats and channels over a date window; read through either
  the Teams desktop app or Teams for the web.
- **Can't:** download someone else's recording (Teams lets only its owner
  download it; Claude says so and the extension never works around it); give
  join and leave times (only who joined); list every attendee of a very large
  meeting (Teams truncates that list); export chat attachments or system
  messages; read personal (teams.live.com) accounts. A channel export's
  members are the people who posted in the window, because Teams exposes no
  channel roster.
- **Speaker names** come from the Teams window, which shows one account at a
  time, so the extension may switch the window to the meeting's account and
  back. When names can't be recovered, the complete transcript is still
  returned without them. Batch downloads never carry names.
- **Files:** transcripts and chats come back into the chat unless you ask for
  a file. Files (and recordings, always) are written only to
  `~/.teams-transcripts/output`, and Claude gives you the full path.
- **Restarts:** the desktop app may need restarting with debugging switched
  on. That is never done during a live or undetermined call unless you turn
  on **Allow forced Teams restarts** and ask for it. Teams for the web never
  closes or restarts anything: it runs in its own Chrome or Edge window,
  minimised in the background.
- **Updates:** once a day it checks its landing page for a newer version.
  When there is one, Claude shows an update card: **Update now** downloads
  it, checks it and opens Claude Desktop's install dialog, and it is
  installed only when you click **Install** there. **Not now** hides that
  version for a week. Claude can't update it for you. Turn **Check for
  updates** off in the extension's settings to stop the daily check.
- **Your data:** transcripts, chat and participant details that Claude reads
  become part of your conversation with Claude, under your Claude account's
  terms. The extension itself makes no Vertex AI or other model calls.

## What you need

| Requirement | Details |
|---|---|
| Operating system | macOS only. Tested on Apple silicon; Intel Macs are expected to work but haven't been verified yet |
| Claude Desktop | 2.7032.0 or later |
| Teams | The Microsoft Teams desktop app, or Google Chrome or Microsoft Edge for Teams for the web (the extension starts its own window, with its own profile, separate from your everyday browser) |
| Account | A Microsoft 365 work account signed in to Teams. Personal (teams.live.com) accounts are never used |
| Network at install | Claude Desktop downloads Python and the extension's locked dependencies the first time it starts it (about a minute) |
| Capturing slides | Firefox signed in to the recording's tenant, and macOS Screen Recording and Accessibility permission (the first capture may ask) |

## What you can ask

**Ask in plain English.** Claude picks the right tool, and reads every work
account and tenant you are signed in to at once, without switching Teams.
Times are UTC. For example:

| Topic | You ask | Claude does |
|---|---|---|
| Status | "Is Teams ready?" | checks Teams and, if needed, starts it and asks you to sign in |
| Meetings | "List my meetings with transcripts from the last week" | lists meetings across every signed-in account and tenant |
| Transcript | "Get the transcript of this morning's design review, with speaker names" | downloads the transcript, the meeting chat and the participants |
| Participants | "Who was invited to Tuesday's planning meeting, who spoke and who joined?" | reads the invitation, the speakers and the call's attendees |
| Meeting chat | "What was posted in the chat during yesterday's all-hands?" | the meeting chat, with reactions |
| Recording | "Save the recording of Monday's kickoff" | downloads the `.mp4` (a recording you own) to `~/.teams-transcripts/output` |
| Batch | "Download every transcript from last week to files" | one file per meeting (fast, without speaker names) |
| Chats | "Export my chat with alice@contoso.com since 1 September" | that person's chat, with members and reactions |
| Channels | "Export the Announcements channel in the Product team for the last 30 days" | the channel's messages |
| Tenants | "Which tenants and accounts can you read?" | the signed-in tenants and accounts |
| Slides | "Capture the slides from that meeting's recording" | its presentation slides as PNGs, timed to the transcript |
| Deck | "Build a notes-page deck from that meeting folder" | a PDF (or PPTX) with each slide's transcript and chat |
| Which Teams | "Use Teams for the web" / "Use the desktop app" | switches Teams for that request |
| Updates | "Check for updates to Teams Transcripts" | shows the update card |

## Available tools

| Tool | What it does | Changes anything? |
|---|---|---|
| `teams_status` | Whether Teams is ready, which accounts are signed in, and the one thing you must do next (sign in, start the app, confirm interrupting a call) | No |
| `teams_start` | Starts Teams for the web in its own Chrome or Edge window, or restarts the desktop app with debugging (never during a call); brings the sign-in window to the front and can wait for you to sign in | Starts or restarts Teams |
| `restart_teams` | Safely restarts the Teams desktop app with debugging switched on; refused during a live or undetermined call unless forced restarts are allowed | Restarts Teams |
| `list` | Meetings with transcripts and recordings, chats and channels, or the signed-in tenants | No |
| `download` | One meeting: transcript (cues, turns or text), meeting chat and participants; or its recording with `recording=true` | Only with `output_path`: a file in the output folder (a recording always is one) |
| `download_batch` | Every transcript in a date window, one file per meeting, without speaker names | Writes files in the output folder |
| `download_chat_messages` | A person's chat, a chat by thread id or Teams link, a channel, or all chats, over a date window | Only with `output_path` |
| `download_slides` | A recording's presentation slides, as PNGs and a manifest timed to the transcript | Writes a folder in the output folder |
| `build_deck` | A notes-page PDF or picture PPTX from a captured meeting folder, each slide with its transcript and chat | Writes files in the output folder |
| `deck_slide_images` | Captured slide images, in small batches, for building an editable deck | No |
| `check_for_updates` | Checks the landing page for a newer version and shows the update card | No |
| `install_update`, `snooze_update` | Used only by the update card's Update now and Not now buttons; hidden from Claude | Opens Claude Desktop's install dialog |

The output folder is `~/.teams-transcripts/output`. Every tool that reads
Teams takes `target` (`desktop` or `web`): Claude passes it when you name one,
otherwise the extension's default applies. `build_deck` and
`deck_slide_images` work on files already captured, so they don't.

## Settings

All optional. In Claude Desktop, open Settings > Extensions > Teams
Transcripts > Configure.

| Setting | What it does |
|---|---|
| Which Teams | **Default to Teams for the web (off = Teams desktop app).** Off: read the Teams app on this Mac. On: read Teams for the web in its own Chrome or Edge window (you sign in once). Asking for the other ("use Teams for the web", "use the Teams desktop app") uses it for that request |
| Web window | **Hide the web window off-screen while it reads speaker names (macOS).** Teams for the web stays minimised, but the meeting's window is shown for a few seconds to read speaker names. On: it is parked in the bottom-right corner with only a sliver showing. Needs Accessibility permission for Claude |
| Tenant | **Default tenant (advanced).** The tenant to read when more than one is signed in |
| Accounts | **Signed-in accounts to use (optional).** Only these identities are read, for example `alice@contoso.com:contoso.onmicrosoft.com, bob@fabrikam.com:fabrikam.onmicrosoft.com`. Blank: every signed-in account |
| Ignored accounts | **Signed-in accounts to ignore (optional).** Identities left out unless a question names them; they are shown as ignored, never as failures |
| Glossary | **Glossary corrections (optional).** `FROM=TO` fixes for transcript text, for example `Fabrikan=Fabrikam, claw desktop=Claude Desktop`. Every change is listed in the result's `corrections` |
| Forced restarts | **Allow forced Teams restarts (advanced, off by default).** Lets Claude restart the desktop app, when you ask, even if a call may be live |
| Updates | **Check for updates.** On by default. Once a day, checks the landing page for a newer version and offers it in the chat; nothing is installed without your click |

## Security and privacy

- **Your own Teams session.** The extension reads through the Teams session
  you are already signed in to: the desktop app, or its own browser profile
  for Teams for the web. It needs no password, token or app registration,
  never types your credentials, and never reads or prints Teams' tokens.
  Signing in, MFA included, is always yours to do.
- **A separate browser for Teams for the web.** It uses its own profile in
  `~/.teams-transcripts/web-profile`, with one saved profile per account,
  never your everyday Chrome or Edge.
- **No AI calls of its own.** The extension makes no Vertex AI or other model
  calls; Claude Desktop supplies the assistant. Its errors carry fixed codes
  only, never meeting content.
- **Files stay in one folder.** It writes only into
  `~/.teams-transcripts/output` (created private to you, mode `0700`),
  refuses symlinks, and never replaces a file unless asked. It never deletes
  what it writes.
- **Careful restarts.** Before restarting the desktop app it checks for a
  live call (audio activity and a visible Leave button) and refuses when a
  call is live or can't be ruled out. Forced restarts need the **Allow forced
  Teams restarts** setting, which is off by default.
- **No workarounds.** A recording Teams won't let you download is reported,
  never fetched another way.
- **Checked installs and updates.** The installer checks the download's
  SHA-256 before anything changes, and `--status` never prints an account or
  an email address. Once a day the extension asks the landing page which
  version is the latest (one small download from GitHub Pages; nothing about
  you is sent) and keeps the day's result in
  `~/Library/Caches/teams-transcripts/update-check.json`. Its update card
  checks the new version's SHA-256 too, and only opens Claude Desktop's
  install dialog: nothing is installed until you click Install there.
- **Install only official releases.** An `.mcpb` is a full program that can
  read Teams and restart it: install it only from this project's landing
  page or releases.
- Report security problems as SECURITY.md describes. Never
  include credentials, tokens or meeting content (transcripts, chat,
  recordings) in a report.

## Troubleshooting

- **The installer says "Checksum mismatch — nothing was installed".** A new
  release can take up to about 10 minutes to reach every server. Wait, then
  run the install command again.
- **The installer says Claude Desktop is missing or too old.** Install or
  update Claude Desktop (2.7032.0 or later), open it once, then run the
  install command again.
- **Claude has no Teams tools.** Switch the extension on (Settings >
  Extensions), then start a new chat: chats opened while it was off never see
  its tools.
- **`--status` says `Server: failed`.** Run the install command again. If it
  keeps failing, `~/Library/Logs/Claude/mcp-server-Teams Transcripts.log`
  says why.
- **The extension shows as failed after install.** It needs network access
  the first time, to download Python and its dependencies. Run the install
  command again; `~/Library/Logs/Claude/main.log` says what went wrong.
- **Claude asks you to sign in to Teams.** Sign in in the window it brought
  to the front (MFA included); Claude waits and carries on. The sign-in is
  remembered.
- **Claude says a call is in progress, or that it can't tell, and it hasn't
  restarted Teams.** The desktop app needs a restart with debugging, which is
  never done during a live call or one that can't be ruled out (a call is
  recognised by Teams' own "call in progress" power assertion or a call
  window; if neither can be read, Claude says it can't tell). Ask again
  after the call, or ask Claude to use Teams for the web, which never
  restarts anything.
- **Claude says Teams for the web is "still opening a page for each tenant".**
  Teams for the web shows one organisation per page, so the first request
  after a fresh sign-in opens a page for each of yours. That can take about
  90 seconds; ask again and Claude carries on. If a request times out while
  this is happening, the error says so (`preflight_timeout`): ask "Is Teams
  ready?" and then repeat the request.
- **Claude says one of your accounts isn't signed in.** The other accounts
  are still readable. Say "sign that account in" and Claude opens its
  sign-in window; it never opens one you didn't ask for.
- **"That tenant isn't signed in here" (`TENANT_NOT_FOUND`).** Sign in to
  that tenant in Teams, or ask "Which tenants can you read?" and pick one.
- **Claude can't find a meeting (`MEETING_NOT_FOUND`).** Check the date, or
  ask Claude to list your meetings and pick one from the list.
- **A transcript has no speaker names.** Names come from the Teams window.
  Ask for that one meeting (batch downloads never have names), and make sure
  its account is signed in to Teams.
- **A recording fails with `download_restricted`.** Teams lets only a
  recording's owner (usually the organiser) download it. Ask them for it.
- **Claude says Teams is busy (`BUSY`).** Another request is using the same
  Teams. Wait a moment and ask again.
- **A Teams for the web window shows "Pick an account".** That account's
  sign-in didn't survive. Ask "Is Teams ready?": Claude brings the window
  forward for you to sign in again. The extension's browser windows stay
  open, minimised, between requests; that is normal.

## Update and remove

- **Update:** when a new version is out, Claude shows an update card in the
  chat (at most once a day); click **Update now**, then **Install** in Claude
  Desktop's dialog, and start a new chat. **Not now** hides that version for
  a week. You can also ask *"Check for updates to Teams Transcripts"*, or run
  the install command again. Your settings are kept. To stop the daily
  check, turn **Check for updates** off in the extension's settings.
- **Remove:** uninstall the extension in Claude Desktop (Settings >
  Extensions). Then run the install command with `bash -s -- --uninstall` in
  place of `bash` to remove the installer's download folder,
  `~/.teams-transcripts-extension` (it asks first; add `--yes` when there is
  no terminal). Neither touches `~/.teams-transcripts`, which holds your
  saved files and the Teams for the web sign-ins: delete it yourself if you
  want those gone.

---

<sub>Generated from the project README for version 1.22.0. It is rebuilt automatically with each release.</sub>
