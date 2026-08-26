# Operating Procedures

## 1. Normal weekly Culvert import

Use this for the normal current guild week.

1. Post all screenshots in `#cat-eating-a-culvert`.
2. Screenshots may span several Discord messages.
3. Right-click the first screenshot message.
4. Select **Apps > Start Culvert Import**.
5. Choose **Current week**.
6. Enter a Tiger week date. Wednesday and Thursday boundary dates can resolve to the same stored Thursday week. Current mode accepts only the current Tiger week or the most recently completed Tiger week.
7. Check the resolved Tiger week shown by the bot.
8. On each middle screenshot message, use **Add to Culvert Import**.
9. On the last screenshot message, use **Finish Culvert Import**.
10. Wait for the bot to report that the import has been staged in **Import Review**.
11. Open Google Sheets and inspect **Import Review**.
12. Confirm the Week column is the intended Thursday date.
13. Resolve every row marked `REVIEW`.
14. Spot-check a sample of scores, especially high scores, low scores, zeroes, screenshot boundaries, and known OCR Alias players.
15. Use **Tiger Tracker > Confirm week into Weekly Data**.
16. If the import is eligible to reconcile the current roster, review the proposed Active/inactive changes before approving them.
17. Tiger writes Weekly Data and verifies the target week row count and Player IDs.
18. If verification fails, Tiger attempts to restore the previous Weekly Data state and keeps Import Review.
19. After core confirmation succeeds, Tiger marks the Discord import session confirmed and clears Import Review automatically.
20. Dashboard, Culvert History, Data Audit, and Member Profile are refreshed. If a derived view fails to refresh, the confirmation alert names the view needing attention.
21. When ready, post `/weekly` in `#tiger-culvert`.

### Important

Do not manually clear Import Review after a normal successful confirmation. It clears automatically. The main-menu **Clear Import Review** action remains available to deliberately discard staged data or recover from an abandoned import.

---

## 2. Historical Culvert import

Use this for an older guild week.

1. Post or locate all screenshots for the historical week in Discord.
2. Right-click the first screenshot message and use **Start Culvert Import**.
3. Choose **Historical**.
4. Enter a Tiger week date, for example `2 Jul`, `1 Jul`, `2/7`, or another supported Wednesday/Thursday boundary date.
5. Check the resolved Tiger week displayed by the bot.
6. Add middle messages with **Add to Culvert Import**.
7. Use **Finish Culvert Import** on the final screenshot message.
8. Wait for Import Review staging.
9. Open **Import Review** and check the Week column before doing anything else.
10. Resolve any REVIEW rows and spot-check OCR results.
11. Use **Confirm week into Weekly Data**.
12. Read the explicit historical date confirmation and verify the exact target date.
13. Approve only if the date is correct.
14. Historical imports do not change the current Active roster, current Last Seen state, or Culvert role state.
15. Import Review clears automatically only after the core confirmation succeeds.

Historical matching uses current players, former/inactive players, genuine player aliases, OCR Aliases, unique truncated matches, and unique fuzzy matches.

---

## 3. Weekly Culvert role workflow

### During the week or for testing

Use **Culvert Zeros - Add Only** when the screenshot set may be partial.

1. Right-click the message containing zero-score screenshots.
2. Select **Apps > Culvert Zeros - Add Only**.
3. Review the public role preview.
4. Confirm vacation-exempt or unlinked exceptions if shown.
5. If the preview is correct, the creating admin can select **Apply Culvert Role**.
6. Add Only can add roles but cannot remove them.

### Complete weekly zero list

Use **Culvert Zeros** only when the screenshot set is the complete weekly zero list.

1. Put the complete zero-score screenshots in one Discord message if possible.
2. Right-click the message and choose **Apps > Culvert Zeros**.
3. Wait for OCR and full active-roster role checks.
4. Review `Would add`, `Already has role`, `Vacation exempt`, and `Would remove` counts.
5. If a safety lock appears, do not try to work around it. Correct the source issue and run a fresh full scan.
6. The admin who created the preview selects **Apply Culvert Role**.
7. Apply rechecks the current roster and affected Discord members before writing roles.

Vacation-exempt zero players are not supposed to carry the reminder role during their excused Tiger weeks.

---

## 4. Manual Culvert reminder

Use this whenever admins want to remind members who currently have the configured Culvert role.

1. Run `/culvertreminder`.
2. The command acknowledgement is private.
3. Tiger posts the public reminder in the channel configured by `CULVERT_REMINDER_CHANNEL_ID`.
4. The reminder mentions the role configured by `CULVERT_PENDING_ROLE_ID`.
5. The dynamic deadline points to Thursday **9:50 AM Brisbane time**.

The Discord role can be renamed without changing the role ID or Cloudflare variable.

---

## 5. Vacation procedure

1. A member posts their vacation/leave information in the guild vacation channel.
2. An admin confirms the approved dates.
3. Run `/vacation add`.
4. Select the member.
5. Select `MM/DD/YYYY` or `DD/MM/YYYY` for numeric dates.
6. Enter start and end dates.
7. Add an admin note only if useful.
8. The successful vacation record is posted publicly.
9. Check `/vacation list` if needed. Use `public:true` only when the list should be visible to everyone.
10. `/whois` and `/me` can show a current/upcoming exemption.
11. Tiger automatically excludes approved vacation weeks from normal Culvert attendance-miss logic and zero-score role assignment.
12. If plans change, run `/vacation remove member` to cancel current/upcoming exemptions. Past records remain for history.

---

## 6. Import recovery

### Check an import

Run `/culvertimport status`.

Use `public:true` only if the status should be visible to the channel.

### OCR failed or import is waiting for OCR

Run `/culvertimport process` when status is `READY_FOR_OCR` or `OCR_FAILED`.

The saved multi-message session is reused. You do not need to re-add the screenshot messages unless the session itself was cancelled or lost.

### Wrong import session

Run `/culvertimport cancel`, then start a new import from the correct first screenshot message.

### Import Review already contains data

Do not overwrite it. Either:

- finish/confirm the existing staged import, or
- deliberately use **Tiger Tracker > Clear Import Review** to discard it

---

## 7. Manual CSV fallback

Use this only if the Discord screenshot importer is unavailable.

1. Create the CSV from screenshots using the strict extraction prompt on **Start Here**.
2. Use **Tiger Tracker > Import weekly CSV**.
3. Select the correct Tiger week and Flag Race availability.
4. Review Import Review.
5. Resolve all REVIEW rows.
6. Confirm into Weekly Data.
7. Import Review clears automatically after the core confirmation succeeds.

---

## 8. Adding or resolving a player from Import Review

### Existing player

1. Select the Import Review row.
2. Choose/correct the Matched IGN if required.
3. Use **Review / Maintenance > Resolve selected row to chosen player**.

### New current member

1. Select the unresolved Import Review row.
2. Use **Tiger Tracker > Add selected as active player**.

### Historical/former member

1. Select the unresolved Import Review row.
2. Use **Tiger Tracker > Add selected as former player**.

The guided **Add selected row as new player** action remains available under Review / Maintenance as an optional fallback.

---

## 9. Weekly public Discord report

The normal weekly public post is:

`/weekly`

Post it in `#tiger-culvert` when the week is ready.

`/leaderboard`, `/summary`, and `/milestones` remain available as optional admin-triggered public views for regular chat when needed.

---

## 10. Rebuilding Sheet views

Normal successful confirmation refreshes Dashboard, Culvert History, Data Audit, and Member Profile automatically.

Use manual rebuild actions only when:

- data was manually repaired in Weekly Data
- a confirmation reports that a derived view needs attention
- a layout/view appears stale

Weekly Data is the source-of-truth data table. There is no normal procedure to rebuild or wipe Weekly Data.
