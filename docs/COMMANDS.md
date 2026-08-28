# Discord Command Reference

## Permission model

### Member access

`/me` has its own access rules.

Inside the Tiger Discord server, it is available to a member with the configured Tiger role, a Tiger Admin, or the configured owner.

Outside the Tiger server, the Discord account must be linked to an Active player in the Players sheet. The configured owner bypasses the Active requirement.

### Admin access

The following count as admin access:

- configured Tiger Admin role
- configured Discord Admin role
- configured owner

When an admin command is used outside the Tiger server, the Worker checks the caller's roles in the Tiger guild through Discord's REST API.

## Slash commands

### `/me [public]`

Shows the caller's Tiger member profile.

The profile can include current IGN, weekly job, Tiger Rank, same-job rank, recent Culvert performance, personal best data, participation information, Discord link state, and separate On Vacation / Upcoming Vacation date ranges with approximate duration. Tiger Rank and same-job rank use the complete newest Weekly Data roster, including members with a Culvert score of 0; equal scores share the same rank.

**Permission:** Member access rules above.

**Visibility:** Private by default. Set `public:true` to show it to the channel.

---

### `/whois member [public]`

Shows another linked member's Tiger profile.

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

---

### `/vacation list [member] [public]`

Lists Tiger vacations in separate **On Vacation** and **Upcoming Vacations** sections. Current entries are ordered by least time remaining and show an approximate number of days left; upcoming entries are ordered by start date and show an approximate number of days until they start. An optional member can filter the list.

**Permission:** Admin.

**Visibility:** Private by default. Set `public:true` to show the list in the channel.

---

## Culvert reminder

### `/culvertreminder`

Posts the standard Culvert deadline reminder in the configured reminder channel and mentions the configured Culvert role.

The dynamic Discord timestamp targets Thursday **9:50 AM Brisbane time**, 10 minutes before weekly reset.

**Permission:** Admin.

**Visibility:** The command acknowledgement is private. The actual reminder message in the configured reminder channel is public and mentions the role.

**Configuration:**

- `CULVERT_REMINDER_CHANNEL_ID`
- `CULVERT_PENDING_ROLE_ID`

The environment variable retains the historical `PENDING` name, but the Discord role itself can be renamed freely because the bot uses the role ID. The same reminder is also posted automatically after a successful Culvert Reminder Apply.

---

### `/culvertreset confirm:true`

Manually runs the same whole-guild Culvert Reminder role cleanup used after the weekly reset.

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

After post-write verification confirms the affected members, Tiger marks the Apply successful and automatically posts the same reminder used by `/culvertreminder`. No second confirmation is required. If the reminder post itself fails, the role changes remain in place and Tiger pings the issuing admin with the failure reason.

**Progress and failures:**

The public interaction message is edited as OCR, Discord checks and role writes progress. A watchdog warns and pings the issuing admin if visible progress stops for about 90 seconds. Terminal failures ping the issuing admin and include a diagnostic.

**Permission:** Admin.

**Visibility:** Permission/initial validation failures are private. Successful progress, role preview, Apply/Cancel result, stall warnings and role-workflow failures are public in the source channel.

### Weekly role reset

At Thursday **10:05 AM Brisbane time**, Tiger automatically removes the Culvert Reminder role from all current holders and verifies the final guild role state. If the cleanup cannot start, fails, or still has role holders after the watchdog window, Tiger posts a detailed alert in the latest Culvert Reminder source channel and mentions that issuing admin. If no prior source context is available, the configured reminder channel/Tiger Admin role is used as a fallback.

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


> `/culvertreset confirm:true` posts its progress and final result publicly in the channel so other admins can see the reset. Permission and validation errors remain private.
