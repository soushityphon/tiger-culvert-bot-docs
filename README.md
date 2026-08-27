# Tiger Culvert Bot Admin Guide

This repository is the public-safe admin guide for the Tiger Culvert Bot and Google Sheets tracker.

It explains how to use the bot, add current and historical Culvert data, manage vacations, run the Culvert Reminder workflow, post weekly reports, and recover from common workflow issues.

No bot source code, secrets, API details, deployment settings, or private configuration are included here.

## Main admin workflows

### Add current-week Culvert data

1. Post the full weekly Culvert screenshot set in `#cat-eating-a-culvert`.
2. Right-click the first screenshot message and choose **Apps > Start Culvert Import**.
3. Choose **Current week** and enter a date for the intended Tiger week.
4. Check the resolved week shown by the bot.
5. Use **Add to Culvert Import** on each middle screenshot message.
6. Use **Finish Culvert Import** on the final screenshot message.
7. Wait for Tiger to stage the OCR result in **Import Review**.
8. Check the **Week** column, resolve any `REVIEW` rows, and spot-check the OCR result.
9. Use **Tiger Tracker > Confirm week into Weekly Data**.
10. After a successful confirmation, Import Review clears automatically.
11. Post `/weekly` in `#tiger-culvert` when the weekly report is ready.

### Add historical Culvert data

1. Post or locate the complete screenshot set for the historical week.
2. Start the import with **Apps > Start Culvert Import**.
3. Choose **Historical** and enter the intended Tiger week date.
4. Add middle screenshot messages with **Add to Culvert Import** and finish with **Finish Culvert Import**.
5. Check **Import Review**, especially the Week column and any `REVIEW` rows.
6. Use **Tiger Tracker > Confirm week into Weekly Data** and verify the historical date before approving.

Historical imports do not change the current guild roster or Culvert role state.

### Culvert Reminder

1. Supply the complete zero-score screenshot set for the week.
2. Right-click the message and choose **Apps > Culvert Reminder**.
3. Wait for Tiger to finish OCR, roster matching, and Discord role checks.
4. Review the role preview before selecting **Apply Culvert Role**.
5. Tiger applies the changes safely, verifies the final state, and automatically posts the standard Culvert reminder after a successful Apply.

Use `/culvertreminder` when the reminder needs to be posted manually. Use `/culvertreset confirm:true` when the Culvert Reminder role needs to be removed from all current holders manually.

### Vacations

- `/vacation add ...` records an approved vacation.
- `/vacation remove member` removes current or upcoming vacations while preserving historical records.
- `/vacation list [member] [public]` separates **On Vacation** and **Upcoming Vacations**.
- `/me` and `/whois` can show **On Vacation** and **Upcoming Vacation** with the date range and approximate duration.
- Approved vacation weeks are excluded from normal Culvert attendance-miss handling and zero-score reminder-role assignment.

### Reports and member lookups

- `/weekly [week]` posts the normal weekly public report.
- `/summary [week]`, `/leaderboard [week]`, and `/milestones [week]` provide optional weekly views.
- `/me [public]` shows your profile.
- `/whois member [public]` lets admins view another member's profile.

For full details:

- [Discord commands and permissions](docs/COMMANDS.md)
- [Operating procedures](docs/PROCEDURES.md)
- [Google Sheets menu guide](docs/GOOGLE_SHEETS_MENU.md)

## Visibility conventions

Some information commands are private by default and support `public:true` when the result should be visible to the channel.

Actions that change data or roles use fixed visibility rules. Their confirmation or preview may be public where other admins need to see the current state.

## Date handling

Tiger supports both numeric date orders when the command provides a date-format choice:

- `MM/DD/YYYY`
- `DD/MM/YYYY`

Week-based commands also accept readable dates such as `2 Jul` or `Jul 2`.

For Tiger week lookup, adjacent Wednesday/Thursday dates can resolve to the same stored Thursday week. Always check the resolved week shown by the bot or the **Week** column in Import Review before confirming historical data.

## Safety rules

- OCR stages data in **Import Review** before Weekly Data is changed.
- Historical imports do not change the current guild roster or Culvert role state.
- Partial zero-score scans never remove roles.
- Complete zero-score scans only allow removals after safety checks pass.
- Approved vacation weeks are excluded from normal Culvert attendance-miss handling.
- Successful week confirmation verifies the Weekly Data write before clearing Import Review.

## Who should use this guide

This guide is intended for trusted Tiger admins who operate the bot and tracker.

Because this repository can be public, it deliberately leaves out private implementation and configuration details.
