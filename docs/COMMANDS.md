# Discord Command Reference

## Permission model

### Member access

`/me` has its own access rules.

Inside the Tiger Discord server, it is available to a member with the configured Tiger role, a Tiger Admin, or the configured owner, but the linked Players row must also be marked Active.

Outside the Tiger server, the Discord account must be linked to an Active player in the Players sheet. `/me` has no owner or admin bypass for the Active requirement.

### Admin access

The following count as admin access:

- configured Tiger Admin role
- configured Discord Admin role
- configured owner

When an admin command is used outside the Tiger server, the Worker checks the caller's roles in the Tiger guild through Discord's REST API.

## Failure and recovery messages

Substantial Tiger operations use a short `Reference` such as `IMP-*`, `CUL-*`, `CRR-*`, or another workflow-specific ID. The same reference is used in Tiger's private lifecycle log so Soushi can trace the exact run without asking an admin to inspect infrastructure.

When Tiger reports a failure, read the outcome and follow the next action before doing anything else. Normal replies keep internal state and retry labels in private logs:

- If Tiger explicitly says the action is safe to retry, retry only the named action and only as instructed.
- If Tiger says to wait, do not start a second copy of the workflow. Use the stated status/check command when one is provided.
- If Tiger says not to retry or to send the details to Soushi, stop there and pass Soushi the reply and reference.
- Do not try to repair GitHub, Cloudflare, Worker bindings, source code, or private logs. Those systems are handled by Soushi.
- Follow the explicit recovery instruction; do not infer that you should edit the tracker or Discord manually from an error description.

If Discord itself shows only **The application did not respond** and Tiger provided no state-aware message, do not assume a write command is safe to repeat. For Culvert imports, use `/culvertimport status` first. For other write/role workflows, pass the command name, approximate time, and any visible Tiger reference to Soushi before repeating the action unless the workflow has a documented read-only state check.

---

## Slash commands

### `/8duck question`

Ask Frank a short yes-or-no question (1 to 180 characters). Frank repeats the question in a public reply and gives one playful duck verdict. His 80 varied answers give a yes 45% of the time, uncertainty 20% of the time and a no or wait 35% of the time. Frank does not ask for a follow-up report. Frank leaves Unicode and Discord custom emojis out of the quoted question. If nothing is left, he asks for a question with words. The question cannot ping other people or alter Discord formatting. It is a guild joke, not a claim about MapleStory odds.

**Permission:** Tiger member, Tiger Admin or configured owner in the Tiger server.

**Visibility:** Public in the invoking channel. Invalid questions and access errors are private.

---

### `/luckychannel`

Frank picks one shared Kronos channel from 1 to 40 for each UTC day. The public result resets at 00:00 UTC. **Worked** and **Didn't work** record your report for that day. Tap the other choice to change it, or tap your current choice again to withdraw it. Feedback replies are private. Yesterday's buttons cannot affect today.

Later public replies may include a varied Frank line about today's reports. A single named report uses a freshly checked Tiger member's Discord mention with notifications suppressed. Frank uses a count if that check fails. These are self-reported stories, not proof that a channel changes drop rates. Picks and votes live in a dedicated Durable Object, separate from the Culvert tracker.

**Permission:** Tiger member, Tiger Admin, Discord Admin or configured owner in the Tiger server.

**Visibility:** The channel is public in the invoking channel. Feedback and access errors are private.

---

### `/me [public]`

Shows the caller's Tiger member profile. If the linked Players row is not Active, Tiger returns a private inactive-profile message and does not show profile data. If the Discord account is not linked, Tiger tells the member to ask a Tiger Admin to use `/link`.

The profile can include current IGN, weekly job, Tiger Rank, same-job rank, latest and personal-best scores, change from the previous result, amount below the personal best, a five-tier recent trend when enough history exists, participation, and separate On Vacation / Upcoming Vacation date ranges with approximate duration. Job Rank remains visible even when the member is the only recorded player for that job. When Nexon returns a validated GMS character record, the profile also shows the Nexon character image as the top-right thumbnail. Tiger Rank and same-job rank use the complete newest Weekly Data roster, including members with a Culvert score of 0; equal scores share the same rank. The self-facing view has no footer and does not expose the internal Player ID; admin `/whois` retains only the Player ID in its footer.

If a character is not available through the Nexon rankings lookup, Nexon is unavailable, or the returned identity does not validate against Tiger data, the profile still renders normally with no thumbnail. The Culvert graph remains the main profile image.

**Permission:** Member access rules above.

**Visibility:** Private by default. Set `public:true` to show it to the channel.

---

### `/iranculvert screenshot`

Lets a current Culvert Reminder-role holder submit one **Member Participation Status** screenshot set to **This Week**. The required `screenshot` option uses Discord's normal image attachment picker.

Frank checks the caller's live Tiger membership, single linked Active player and exact IGN. A focused one-image OCR pass reads only the This Week heading, that exact visible IGN row and its Culvert score. The score is checked against every earlier official score and the exact immediately preceding Tiger week. A repeated exact score is treated as suspicious, not proof of fraud. A positive score below 95% of a positive prior-week score must be rerun.

Unreadable, wrong-view, missing-IGN or ambiguous proof returns a private resubmission request with an example. Every readable result is public. Passing proof says the role will come off shortly, then a durable job waits about 30 seconds, rechecks the Tiger week and role, removes the role once and edits the same result with the verified outcome. The command never writes Weekly Data.

**Permission:** Current Culvert Reminder-role holder in Tiger with one linked Active player.

**Visibility:** Reading and identity errors are private. Readable zero, repeated score, below-95% score, pass and service failure are public.

---

### `/whois member [public]`

Shows another linked member's Tiger profile. It uses the same optional validated Nexon character thumbnail and failure-safe fallback as `/me`.

**Arguments:**

- `member`, required Discord member
- `public`, optional boolean

**Permission:** Admin.

**Visibility:** Private by default. Set `public:true` to show it to the channel.

---

### `/weekly [week]`

Posts the combined Tiger weekly Culvert update. This is the normal weekly public post for `#tiger-culvert`.

The optional `week` field supports autocomplete and fuzzy Tiger week lookup.

Accepted examples include:

- `19 Sep`
- `Sep 19`
- `19/9`
- `9/19`

Ambiguous numeric dates are rejected rather than guessed.

**Permission:** Admin.

**Visibility:** Public.

---

### `/summary [week]`

Shows the guild weekly Culvert summary for the selected week or the newest available week.

**Permission:** Admin.

**Visibility:** Public.

**Normal use:** Optional lookup/post in regular chat. The standard weekly channel post is `/weekly`.

---

### `/leaderboard [week]`

Shows the Tiger Culvert leaderboard for the selected week or newest available week.

**Permission:** Admin.

**Visibility:** Public.

**Normal use:** Optional lookup/post in regular chat.

---

### `/milestones [week]`

Shows Tiger weekly Culvert milestones for the selected week or newest available week.

**Permission:** Admin.

**Visibility:** Public.

**Normal use:** Optional lookup/post in regular chat.

---

### `/health`

Runs a private read-only health check across the Worker, Apps Script API, Discord bot authentication, Tiger bot log channel, OCR queue binding, and Gemini configuration. It also shows a compact tracker snapshot, latest import-session status, and the persisted outcome of the latest scheduled Culvert role reset. After the Thursday reset grace window, it flags a missed expected reset when the stored reset date is stale.

**Permission:** Admin.

**Visibility:** Private.

---

### `/link add member player`

Links an Active Tiger player to a Discord member directly from Discord. `player` must be the exact current IGN or Player ID. Tiger verifies that the selected Discord account is currently in the configured Tiger server, refuses duplicate/conflicting identity links, writes both Discord Username and Discord User ID, and verifies the write before reporting success.

**Permission:** Admin.

**Visibility:** Private.

**Safety:** Tiger does not fuzzy-match or use aliases for this write. If the player or Discord account is already linked differently, remove the existing link first.

---

### `/link remove player`

Removes both Discord Username and Discord User ID from the selected Tiger player. This is allowed for Active or inactive players so stale links found by `/linkaudit` can be cleaned up without editing the Sheet.

**Permission:** Admin.

**Visibility:** Private.

---

### `/linkaudit [page]`

Lists show up to eight entries per page, with a shown/total count. Use `/linkaudit page:2` for the next page; the footer shows how many pages are available. Each invocation reads current data, so counts can change if the roster is updated between requests. Access remains admin-only and the response remains private.

Runs a private read-only audit of Tiger roster-to-Discord links. It reports Active players missing a Discord User ID, Active linked players not found in the Tiger Discord, and duplicate Discord User IDs. Inactive players may retain their Discord details in Players; routine inactive links are not listed or counted here. Actual duplicate Discord IDs remain visible even when an affected player is inactive. Moving a Discord account to a different Player still requires explicitly removing the old link first.

**Permission:** Admin.

**Visibility:** Private.

---

### `/nexonaudit [page]`

The completed summary covers the full roster. Review items show eight per page with shown/total and page counts. After the audit finishes, use `/nexonaudit page:2` for another page. **Each request runs a fresh full audit**, so allow the normal processing time and expect counts to change if source data changes. Do not start another page while the previous audit is still running. Out-of-range pages show the last page; all review entries remain reachable.

Runs a private, read-only audit of every Active Tiger player against Nexon GMS character ranking data. Tiger looks up the exact Current IGN, compares the returned character name and Job, checks whether the Nexon Level is plausible against the newest Weekly Data level, and verifies that Nexon returned a trusted character PNG URL.

The audit runs in safe queue batches and edits the same Discord response with progress until the complete Active roster has been checked. It does not change Players, Weekly Data, Import Review, Discord links, or any other tracker data.

If the primary overall-weekly ranking lookup returns no character, Tiger automatically tries the legacy job-ranking lookup before reporting `NOT_FOUND`. Identity, Job, Level and trusted-image validation remain unchanged. The final audit also reports how many valid matches came from the primary and fallback endpoints.

**Level review rule:** Nexon being lower than Tiger, or more than 10 levels ahead, is flagged for admin review. This is identity-quality checking only; Nexon does not become authoritative for Tiger levels.

**Permission:** Admin.

**Visibility:** Private.

---


### `/registercommands`

Re-registers the complete Tiger global Discord command set with Discord. Use this only after a deployment that changes command names, options, or context-menu registrations.

**Permission:** Admin. The request must arrive as a valid signed Discord interaction and then pass the normal Tiger Admin / Discord Admin / owner permission check.

**Visibility:** Private.

**Security:** This is the supported registration path. The old unauthenticated public registration URL has been retired and can no longer trigger command registration.

---

## Vacation commands

### `/vacation add member date_format start end [note]`

Records an approved vacation exemption.

**Arguments:**

- `member`, required Discord member
- `date_format`, required, `MM/DD/YYYY` or `DD/MM/YYYY`
- `start`, required
- `end`, required
- `note`, optional admin note stored in the sheet

Tiger converts the dates to canonical values and stores the exact Tiger weeks covered by the exemption. The Discord confirmation shows the vacation date range with an approximate total duration; detailed excused-week and week-ending rows are not shown in the confirmation.

**Permission:** Admin.

**Visibility:** Successful confirmation is public. Input validation failures are private.

**Effects:**

- appears in member profile information while current/upcoming
- excused zeroes do not count as normal attendance misses
- zero-score role scans exclude the member during excused weeks
- Culvert Reminder can remove an existing role if the member is not in the supplied desired set and all safety checks pass

---

### `/vacation remove member`

Cancels current and upcoming approved vacation exemptions for the selected member. Historical records remain in the sheet as cancelled records rather than being deleted.

**Permission:** Admin.

**Visibility:** Private.

**Recovery after an uncertain write:** Run `/vacation list` for the same member before repeating a vacation write. For add, if the exact dates are already listed, do not add them again; if they are absent, the same add can be repeated. For remove, if no current/upcoming vacation remains, do not repeat the removal; if the intended records remain, repeat `/vacation remove`.

---

### `/vacation list [member] [public]`

Lists Tiger vacations in separate **On Vacation** and **Upcoming Vacations** sections. Current entries are ordered by least time remaining and show an approximate number of days left; upcoming entries are ordered by start date and show an approximate number of days until they start. An optional member can filter the list.

**Permission:** Admin.

**Visibility:** Private by default. Set `public:true` to show the list in the channel.

---

## Culvert reminder

### `/culvertreminder`

Creates or finds the persistent **Send reminder** control in `#admin-culvert-reminder`. The command does not send a role reminder.

The dynamic Discord timestamp targets Wednesday **23:50 UTC**, 10 minutes before weekly reset.

**Permission:** Admin.

**Visibility:** The command reply is private. The button posts a public role ping to the member reminder channel and mentions the role.

**Configuration:**

- `CULVERT_REMINDER_CONTROL_CHANNEL_ID` for the admin button
- `CULVERT_REMINDER_CHANNEL_ID` for the member role ping
- `CULVERT_PENDING_ROLE_ID`

The environment variable retains the historical `PENDING` name, but the Discord role itself can be renamed freely because the bot uses the role ID. Apply creates or finds the admin control and never sends a role ping.

---

### `/culvertreset confirm:true [scheduled_test]`

Manually runs the same whole-guild Culvert Reminder role cleanup used after the weekly reset. Set `scheduled_test:true` to also exercise the scheduled lifecycle wrapper, persistent status, and private outcome logging.

**Permission:** Admin.

**Confirmation:** `confirm:true` is required. `confirm:false` makes no changes.

**Behaviour:** Tiger obtains a complete guild-member snapshot, removes the role in safe queue batches, edits the public command response with progress, then performs a complete final verification. If the reset fails or stalls, Tiger also pings the issuing admin in the channel where the command was run and includes the diagnostic reason.

**Visibility:** Progress and successful completion are public in the source channel so other admins can see that the reset has been run. Permission and validation failures remain private. Failure/stall alerts are also public in the source channel so they cannot be missed.

---

## Culvert import administration

### `/culvertimport status [public]`

Shows the caller's latest Culvert import session, including mode, Tiger week, message count, screenshot count, and current status.

**Permission:** Admin.

**Visibility:** Private by default. Set `public:true` to show the status in the channel.

Common statuses include:

- `PENDING_MODE`
- `OPEN`
- `READY_FOR_OCR`
- `OCR_RUNNING`
- `OCR_FAILED`
- `REVIEW_READY`
- `CONFIRMED`
- `CANCELLED`

---

### `/culvertimport process`

Processes a saved import that is waiting in `READY_FOR_OCR`, or retries one in `OCR_FAILED`.

This is mainly a recovery command. New imports normally start OCR automatically when **Finish Culvert Import** is used.

**Permission:** Admin.

**Visibility:** Private.

---

### `/culvertimport cancel`

Cancels the caller's open import session.

Use this if the wrong week/mode was selected or the screenshot collection should be abandoned.

**Permission:** Admin.

**Visibility:** Private.

---

# Right-click Apps commands

## `Culvert Reminder`

Processes the zero-score screenshots supplied by the Tiger Admin and treats those supplied screenshots as the complete desired Culvert Reminder list. Tiger does not try to decide whether the admin forgot to upload a screenshot.

**Processing integrity:**

- counts every supplied image
- processes OCR in batches with six visible numbered workflow steps; each numbered stage uses `ETA for this step` so the estimate cannot be mistaken for the whole workflow
- retries transient OCR failures without reprocessing successful screenshots
- requires every supplied screenshot to process safely before Apply is available
- blocks Apply when a zero-score row needs review or a required Discord check is unresolved
- respects vacation exemptions
- checks the current Discord role state, preferring a bulk guild-member snapshot for speed and falling back to conservative targeted checks if needed
- previews additions, already-correct members, vacation exemptions and removals
- creates a 30-minute creator-bound Apply plan

**Apply rules:**

Only the admin who created the preview can use Apply/Cancel. Apply re-verifies admin access, current roster/link fingerprint and affected Discord role state. Adds happen before removals. If any required add fails, removals are skipped.

After post-write verification confirms the affected members, Tiger marks Apply successful and prepares the persistent **Send reminder** control in `#admin-culvert-reminder`. Apply does not ping the role. Only a Tiger Admin or Discord Admin can use the button. It posts to the configured member channel without checking the control's week or the number of role holders. The shared message displays the most recent confirmed sender and time. Each click is independent; an uncertain post is not retried.

**Progress and failures:**

The public interaction message is edited as OCR, Discord checks and role writes progress. A watchdog warns and pings the issuing admin if visible progress stops for about 90 seconds. Terminal failures ping the issuing admin and include a diagnostic.

**Permission:** Admin.

**Visibility:** Permission/initial validation failures are private. Successful progress, role preview, Apply/Cancel result, stall warnings and role-workflow failures are public in the source channel.

### Weekly role reset

At Thursday **00:05 UTC**, Tiger automatically removes the Culvert Reminder role from all current holders and verifies the final guild role state. It records started, queued, succeeded or failed state with a run ID, times and safe counts, and shows the latest result in `/health`. A **00:15 UTC** safety cron retries when the primary run is absent or failed, but does not duplicate a cleanup that is still in progress. An hourly Thursday recovery trigger at `35 0-13 * * THU` checks from 00:35 to 13:35 UTC and runs the same cleanup only when no attempt at all was recorded for the current UTC reset date.

If the cleanup cannot start, fails, or still has role holders after the watchdog window, Tiger posts a detailed alert in the latest Culvert Reminder source channel and mentions that issuing admin. If the normal alert fails, Tiger sends an independent fallback alert to the private bot-log channel. If no prior source context is available, the configured reminder channel/Tiger Admin role is used.

The fast guild-member snapshot requires Discord's privileged **Server Members Intent**. The interactive preview/Apply path can fall back to targeted member checks if the snapshot is unavailable, but the scheduled whole-guild reset needs a complete member snapshot.

---

## `Start Culvert Import`

Starts a full-score multi-message import using the selected Discord message as the first screenshot batch.

Tiger asks the admin to choose:

- **Current week**
- **Historical**

Both choices then ask for a Tiger week date.

Current mode only accepts the current Tiger week or the most recently completed Tiger week. Older dates must use Historical mode.

**Permission:** Admin.

**Visibility:** Private.

---

## `Add to Culvert Import`

Adds another screenshot message to the admin's open import session.

The same message/attachment cannot be added twice. One session currently supports up to 40 screenshots.

**Permission:** Admin.

**Visibility:** Private.

---

## `Finish Culvert Import`

Use this on the last screenshot message.

Tiger first makes sure the selected final message is included, then closes screenshot collection and starts full-score OCR.

The OCR pipeline:

1. processes all screenshots through Gemini
2. preserves successful batches if another batch needs retry
3. matches against Tiger players and aliases
4. reuses OCR Aliases
5. merges safe screenshot overlaps
6. sends conflicting overlaps to REVIEW rather than guessing
7. stages the result in Import Review

**Permission:** Admin.

**Visibility:** Public once processing starts. Tiger edits the same message through four concise progress steps with per-step ETA. Start/Add setup messages remain private. Processing stalls and terminal failures are public and mention the initiating admin; validation/permission failures remain private where possible.

For **Current week**, the successfully confirmed Import Review roster is authoritative regardless of member count. Every imported player is Active, and previously Active players absent from that roster are set inactive automatically. Historical imports never reconcile Active status.

---

# Removed/legacy command names

`/culvertzeros` is removed from global registration.

Old right-click names such as `Scan Culvert` remain only as temporary internal compatibility for Discord clients with stale command caches. They are not registered and should not be documented for normal use.


> `/culvertreset confirm:true` posts its progress and final result publicly in the channel so other admins can see the reset. Add `scheduled_test:true` when testing the automatic wrapper. Permission and validation errors remain private.

## Tiger bot action log

Tiger records top-level Discord bot actions in the configured private bot-log channel. This includes slash commands such as `/me` and `/whois`, admin commands, context-menu actions, Apply/Cancel components, modal submissions, and scheduled Worker triggers. The log records who invoked the action, the source channel/guild, and limited operational details. It deliberately does not copy profile contents, secrets, OCR payloads, or every internal retry/progress edit.

Audit logging is best-effort and is isolated from command execution: a logging failure must not block the user action that triggered it.


### Culvert Reminder OCR handling

Unlisted OCR names do not block Culvert Reminder Apply. Names that do not resolve to the authoritative Players roster are shown in the preview and ignored for role targeting. Known roster rows that remain ambiguous, incomplete scans, or unresolved Discord checks still block Apply.

## Culvert import failure recovery

Current import failures include a short `IMP-...` reference, a concise outcome and the safe next action. Saved session state, retry classifications and diagnostics remain in private logs. Follow the action shown in Discord. Do not ask admins to inspect GitHub, Cloudflare or Apps Script; only Soushi has that access.
