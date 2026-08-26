# Tiger Culvert Bot Admin Guide

This repository is the public-safe admin guide for the Tiger Culvert Bot and Google Sheets tracker.

It explains how to use the bot, run weekly and historical Culvert imports, manage vacations, update the Culvert reminder role, post weekly reports, and recover from common workflow issues.

No bot source code, secrets, API details, deployment settings, or private configuration are included here.

## Quick weekly workflow

1. Post the full weekly Culvert screenshot set in `#cat-eating-a-culvert`.
2. Right-click the first screenshot message and choose **Apps > Start Culvert Import**.
3. Choose **Current week** and enter a date for the intended Tiger week.
4. Check the resolved week shown by the bot.
5. Use **Add to Culvert Import** on each middle screenshot message.
6. Use **Finish Culvert Import** on the final screenshot message.
7. Wait for Tiger to stage the OCR result in **Import Review**.
8. Open Google Sheets and check the **Week** column first.
9. Resolve any `REVIEW` rows and spot-check the OCR result.
10. Use **Tiger Tracker > Confirm week into Weekly Data**.
11. After a successful confirmation, Import Review clears automatically.
12. Post `/weekly` in `#tiger-culvert` when the weekly report is ready.

## Main admin workflows

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
- Vacation-exempt weeks are excluded from normal Culvert attendance-miss handling.
- Successful week confirmation verifies the Weekly Data write before clearing Import Review.

## Who should use this guide

This guide is intended for trusted Tiger admins who operate the bot and tracker.

Because this repository can be public, it deliberately leaves out private implementation and configuration details.
