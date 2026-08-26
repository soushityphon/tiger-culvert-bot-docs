# Tiger Culvert Bot Operating Procedures

## 1. Normal weekly Culvert import

Use this for the normal current guild week.

1. Post all Culvert screenshots in `#cat-eating-a-culvert`.
2. Screenshots can span several Discord messages.
3. Right-click the first screenshot message.
4. Choose **Apps > Start Culvert Import**.
5. Choose **Current week**.
6. Enter a date for the intended Tiger week.
7. Check the resolved Tiger week shown by Tiger.
8. Use **Add to Culvert Import** on every middle screenshot message.
9. Use **Finish Culvert Import** on the final screenshot message.
10. Wait for Tiger to report that the import is staged in **Import Review**.
11. Open Google Sheets and check **Import Review**.
12. Check the **Week** column first and confirm it is the intended Thursday date.
13. Resolve every row marked `REVIEW`.
14. Spot-check several OCR rows, especially high scores, low scores, zeroes, screenshot boundaries, and known commonly misread names.
15. Use **Tiger Tracker > Confirm week into Weekly Data**.
16. If Tiger shows proposed current-roster changes, review them before approving.
17. After a successful core confirmation, Import Review clears automatically.
18. Dashboard, Culvert History, Data Audit, and Member Profile refresh from the confirmed data.
19. Post `/weekly` in `#tiger-culvert` when the weekly report is ready.

### Important

Do not manually clear Import Review after a normal successful confirmation. Automatic clearing is part of the success flow.

Use **Clear Import Review** only when deliberately discarding an incorrect or abandoned staged import.

---

## 2. Historical Culvert import

Use this for an older guild week.

1. Post or locate all screenshots for the historical week in Discord.
2. Right-click the first screenshot message and choose **Start Culvert Import**.
3. Choose **Historical**.
4. Enter a Tiger-week date, for example `2 Jul`, `1 Jul`, `2/7`, or another supported boundary date.
5. Check the resolved Tiger week shown by Tiger.
6. Add middle messages with **Add to Culvert Import**.
7. Use **Finish Culvert Import** on the final screenshot message.
8. Wait for Import Review staging.
9. Open **Import Review** and check the **Week** column before doing anything else.
10. Resolve any `REVIEW` rows and spot-check the OCR result.
11. Use **Confirm week into Weekly Data**.
12. Read the historical date confirmation carefully.
13. Approve only if the target date is correct.
14. Historical imports do not change the current Active roster, current member Last Seen state, or Culvert role state.
15. Import Review clears automatically after successful core confirmation.

### Historical date safety

Wednesday and Thursday boundary dates may resolve to the same stored Thursday week. Always check the resolved week and the Import Review Week column before confirming.

---

## 3. Weekly Culvert reminder-role workflow

### Partial or test scan

Use **Culvert Zeros - Add Only** when the screenshot set may be incomplete.

1. Right-click the message containing zero-score screenshots.
2. Choose **Apps > Culvert Zeros - Add Only**.
3. Review the public role preview.
4. Check vacation-exempt, unlinked, or other exceptions if shown.
5. If the preview is correct, the admin who created it can choose **Apply Culvert Role**.
6. Add Only can add or recognise roles but cannot remove them.

### Complete weekly zero list

Use **Culvert Zeros** only when the screenshots represent the complete weekly zero list.

1. Put the complete zero-score screenshot set in one Discord message if possible.
2. Right-click the message and choose **Apps > Culvert Zeros**.
3. Wait for OCR and role checks.
4. Review the counts for additions, existing role holders, vacation exemptions, and removals.
5. If Tiger shows a safety lock, correct the source issue and run a fresh full scan.
6. The admin who created the preview chooses **Apply Culvert Role**.
7. Tiger rechecks current state before writing roles.

Vacation-exempt zero players should not carry the reminder role during their excused Tiger weeks.

---

## 4. Manual Culvert reminder

Use this whenever admins want to remind members who currently have the Culvert reminder role.

1. Run `/culvertreminder`.
2. The command acknowledgement is private.
3. Tiger posts the reminder publicly in the guild's reminder channel.
4. The reminder mentions the current Culvert reminder role.
5. The dynamic deadline points to Thursday **9:50 AM Brisbane time**.

---

## 5. Vacation procedure

1. A member posts their vacation or leave information in the guild vacation channel.
2. An admin confirms the approved dates.
3. Run `/vacation add`.
4. Select the member.
5. Select `MM/DD/YYYY` or `DD/MM/YYYY` for numeric dates.
6. Enter the start and end dates.
7. Add an admin note only if useful.
8. The successful vacation record is posted publicly.
9. Use `/vacation list` when needed. Add `public:true` only if the list should be visible to the channel.
10. `/whois` and `/me` can show current/upcoming exemptions.
11. Tiger automatically excludes approved vacation weeks from normal attendance-miss handling and zero-score reminder-role assignment.
12. If plans change, run `/vacation remove member` to cancel current/upcoming exemptions.

---

## 6. Import recovery

### Check an import

Run `/culvertimport status`.

Use `public:true` only when the status should be visible to the channel.

### OCR failed or is waiting

Run `/culvertimport process` when the saved session is waiting for OCR or an OCR attempt failed.

The existing session is reused. You normally do not need to re-add the screenshot messages.

### Wrong import session

Run `/culvertimport cancel`, then start again from the correct first screenshot message.

### Import Review already contains staged data

Do not overwrite it blindly.

Either:

- finish and confirm the existing staged import, or
- deliberately use **Tiger Tracker > Clear Import Review** to discard it

---

## 7. Manual CSV fallback

Use this only if the Discord screenshot importer is unavailable.

1. Create the CSV from screenshots using the strict extraction prompt on **Start Here**.
2. Use **Tiger Tracker > Import weekly CSV**.
3. Select the correct Tiger week and Flag Race availability.
4. Review Import Review.
5. Resolve all `REVIEW` rows.
6. Confirm into Weekly Data.
7. Import Review clears automatically after successful core confirmation.

---

## 8. Resolving players from Import Review

### Existing player

1. Select the Import Review row.
2. Choose or correct the Matched IGN if required.
3. Use **Review / Maintenance > Resolve selected row to chosen player**.

### New current member

1. Select the unresolved Import Review row.
2. Use **Tiger Tracker > Add selected as active player**.

### Historical or former member

1. Select the unresolved Import Review row.
2. Use **Tiger Tracker > Add selected as former player**.

---

## 9. Weekly public Discord report

The normal weekly public post is:

`/weekly`

Post it in `#tiger-culvert` when the week is ready.

`/leaderboard`, `/summary`, and `/milestones` remain available as optional admin-triggered public views when useful.

---

## 10. Rebuilding Sheet views

A normal successful confirmation refreshes Dashboard, Culvert History, Data Audit, and Member Profile automatically.

Run manual rebuild actions only when:

- Weekly Data was repaired manually
- Tiger reports that a derived view needs attention
- a view appears stale

Weekly Data is the source-of-truth table. There is no normal procedure to rebuild or wipe Weekly Data.
