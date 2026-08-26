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

The profile can include current IGN, job, recent Culvert performance, personal best data, participation information, Discord link state, and current/upcoming vacation exemption information.

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

## Vacation commands

### `/vacation add member date_format start end [note]`

Records an approved vacation exemption.

**Arguments:**

- `member`, required Discord member
- `date_format`, required, `MM/DD/YYYY` or `DD/MM/YYYY`
- `start`, required
- `end`, required
- `note`, optional admin note stored in the sheet

Tiger converts the dates to canonical values and stores the exact Tiger weeks covered by the exemption.

**Permission:** Admin.

**Visibility:** Successful confirmation is public. Input validation failures are private.

**Effects:**

- appears in member profile information while current/upcoming
- excused zeroes do not count as normal attendance misses
- zero-score role scans exclude the member during excused weeks
- full zero scans can remove an existing Culvert role if appropriate

---

### `/vacation remove member`

Cancels current and upcoming approved vacation exemptions for the selected member. Historical records remain in the sheet as cancelled records rather than being deleted.

**Permission:** Admin.

**Visibility:** Private.

---

### `/vacation list [member] [public]`

Lists current and upcoming Tiger vacations. An optional member can filter the list.

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

The environment variable retains the historical `PENDING` name, but the Discord role itself can be renamed freely because the bot uses the role ID.

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

## `Culvert Zeros - Add Only`

Partial/test zero-score scan.

Use it when the supplied screenshot set may not contain the complete weekly zero list.

**Behaviour:**

- OCRs only supplied screenshots
- matches zero-score Tiger players
- respects vacation exemptions
- can preview/add the Culvert role
- can report members who already have the role
- never previews or applies removals

**Permission:** Admin.

**Visibility:** Permission/validation failures are private. Successful scan progress, role preview, and Apply/Cancel result are public in the channel.

---

## `Culvert Zeros`

Complete weekly zero-score scan.

Use this only when the supplied screenshots represent the complete weekly zero list.

**Behaviour:**

- OCRs the complete zero list
- checks the active linked roster
- respects vacation exemptions
- previews role additions
- previews role removals only after all removal safety gates pass
- creates a 30-minute creator-bound Apply plan

**Permission:** Admin.

**Visibility:** Permission/validation failures are private. Successful scan progress, role preview, and Apply/Cancel result are public in the channel.

### Apply button rules

Only the admin who created the preview can use the Apply/Cancel buttons. Apply re-verifies admin access and checks current roster/link state before changing roles. Adds happen before removals, and removals do not proceed if required adds fail.

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

**Visibility:** Private.

---

# Removed/legacy command names

`/culvertzeros` is removed from global registration.

Old right-click names such as `Scan Culvert` remain only as temporary internal compatibility for Discord clients with stale command caches. They are not registered and should not be documented for normal use.
