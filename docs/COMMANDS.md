# Discord Command Reference

## Permission levels

### Member

`/me` is available to eligible Tiger members and admins.

### Admin

Admin-only commands are restricted to authorised Tiger or Discord admins.

## Visibility

- **Private** means only the person running the command sees the result.
- **Public** means the channel can see the result.
- Commands marked **Private by default** can be made public with `public:true`.

---

# Slash commands

## `/me [public]`

Shows your Tiger profile.

The profile can include IGN, job, recent Culvert results, personal-best information, participation information, Discord link state, and current/upcoming vacation exemptions.

**Permission:** Member  
**Visibility:** Private by default. Use `public:true` to show it in the channel.

---

## `/whois member [public]`

Shows another linked member's Tiger profile.

**Arguments:**

- `member`, required
- `public`, optional

**Permission:** Admin  
**Visibility:** Private by default. Use `public:true` to show it in the channel.

---

## `/weekly [week]`

Posts the combined Tiger weekly Culvert update.

This is the normal weekly public post for `#tiger-culvert`.

The optional week field supports autocomplete and fuzzy Tiger-week lookup.

Examples:

- `19 Sep`
- `Sep 19`
- `19/9`
- `9/19`

If a numeric date is genuinely ambiguous, Tiger asks for a clearer date rather than guessing.

**Permission:** Admin  
**Visibility:** Public

---

## `/summary [week]`

Shows the guild weekly Culvert summary for the selected or newest available week.

**Permission:** Admin  
**Visibility:** Public  
**Normal use:** Optional lookup or post in regular chat.

---

## `/leaderboard [week]`

Shows the Tiger Culvert leaderboard for the selected or newest available week.

**Permission:** Admin  
**Visibility:** Public  
**Normal use:** Optional lookup or post in regular chat.

---

## `/milestones [week]`

Shows weekly Tiger Culvert milestone statistics.

**Permission:** Admin  
**Visibility:** Public  
**Normal use:** Optional lookup or post in regular chat.

---

# Vacation commands

## `/vacation add member date_format start end [note]`

Records an approved vacation exemption.

**Arguments:**

- `member`, required
- `date_format`, required, `MM/DD/YYYY` or `DD/MM/YYYY`
- `start`, required
- `end`, required
- `note`, optional admin note

**Permission:** Admin  
**Visibility:** Successful confirmation is public. Input errors are private.

**Effects:**

- current/upcoming exemptions can appear in member profile information
- excused zeroes do not count as normal attendance misses
- zero-score role scans exclude the member during excused weeks
- a complete zero scan can remove an existing Culvert reminder role when appropriate

---

## `/vacation remove member`

Cancels current and upcoming approved vacation exemptions for the selected member.

Past records remain for history.

**Permission:** Admin  
**Visibility:** Private

---

## `/vacation list [member] [public]`

Lists current and upcoming Tiger vacations.

An optional member filter can limit the result to one member.

**Permission:** Admin  
**Visibility:** Private by default. Use `public:true` to show it in the channel.

---

# Culvert reminder

## `/culvertreminder`

Posts the standard Culvert deadline reminder in the guild's reminder channel and mentions the current Culvert reminder role.

The dynamic deadline points to Thursday **9:50 AM Brisbane time**, 10 minutes before weekly reset.

**Permission:** Admin  
**Visibility:** The command acknowledgement is private. The reminder itself is public in the reminder channel.

The Discord role may be renamed without changing how the command is used.

---

# Culvert import administration

## `/culvertimport status [public]`

Shows your latest Culvert import session, including mode, Tiger week, message count, screenshot count, and status.

**Permission:** Admin  
**Visibility:** Private by default. Use `public:true` to show it in the channel.

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

## `/culvertimport process`

Processes a saved import waiting for OCR, or retries one that failed during OCR.

New imports normally begin OCR automatically when **Finish Culvert Import** is used.

**Permission:** Admin  
**Visibility:** Private

---

## `/culvertimport cancel`

Cancels your open import session.

Use this when the wrong week or mode was selected, or the screenshot collection should be abandoned.

**Permission:** Admin  
**Visibility:** Private

---

# Right-click Apps commands

## `Culvert Zeros - Add Only`

Partial/test zero-score scan.

Use this when the screenshots may not contain the complete weekly zero list.

**Behaviour:**

- scans only the supplied screenshots
- matches zero-score Tiger players
- respects vacation exemptions
- can preview and add the Culvert reminder role
- can report members who already have the role
- never previews or applies removals

**Permission:** Admin  
**Visibility:** Validation failures are private. Successful progress, role preview, and Apply/Cancel result are public in the channel.

---

## `Culvert Zeros`

Complete weekly zero-score scan.

Use this only when the supplied screenshots represent the complete weekly zero list.

**Behaviour:**

- scans the complete zero list
- checks the current linked roster
- respects vacation exemptions
- previews additions
- previews removals only after safety checks pass
- creates a short-lived Apply plan

**Permission:** Admin  
**Visibility:** Validation failures are private. Successful progress, role preview, and Apply/Cancel result are public in the channel.

### Apply rules

Only the admin who created the preview can use its Apply/Cancel buttons. Apply rechecks current state before changing roles.

---

## `Start Culvert Import`

Starts a full-score, multi-message screenshot import using the selected Discord message as the first batch.

Tiger asks you to choose:

- **Current week**
- **Historical**

Both modes ask for a Tiger-week date.

Current mode only accepts the current Tiger week or the most recently completed Tiger week. Older dates should use Historical mode.

**Permission:** Admin  
**Visibility:** Private

---

## `Add to Culvert Import`

Adds another screenshot message to your open import session.

The same message or attachment cannot be added twice. One import session supports multiple Discord messages and up to 40 screenshots.

**Permission:** Admin  
**Visibility:** Private

---

## `Finish Culvert Import`

Use this on the final screenshot message.

Tiger makes sure that final message is included, closes screenshot collection, starts OCR, matches players and aliases, merges safe overlaps, and stages the result in **Import Review**.

Conflicting overlaps are sent to `REVIEW` instead of being guessed.

**Permission:** Admin  
**Visibility:** Private

---

# Legacy names

Old slash and right-click names may briefly remain visible in Discord because of client caching. Use the command names documented above for normal operation.
