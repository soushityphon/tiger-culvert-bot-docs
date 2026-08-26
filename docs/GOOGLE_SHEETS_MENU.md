# Google Sheets Menu Reference

All Google Sheets menu actions rely on spreadsheet access and Apps Script authorisation. Discord role checks do not control Sheet menu access, so access to the spreadsheet itself should be limited to trusted users.

## Tiger Tracker

### `Import weekly CSV`

Manual fallback importer for weekly screenshot-derived CSV data.

The Discord screenshot importer is the normal workflow. Keep CSV import as a fallback.

### `Add selected as active player`

From an Import Review row, creates a new Players record marked Active and resolves the selected review row to that Player ID.

### `Add selected as former player`

From an Import Review row, creates a new Players record marked inactive/former and resolves the selected review row.

Use this for historical players who should not become current guild members.

### `Confirm week into Weekly Data`

Validates Import Review and writes the selected week into Weekly Data.

Core safety steps include:

- every staged row must be `OK`
- each player must resolve to a valid Player ID
- duplicate Player IDs are blocked
- required numeric values must be present
- historical imports show an exact target-date confirmation
- replacing an existing week requires explicit confirmation
- current complete imports can show a roster reconciliation preview
- the target Weekly Data week is re-read and verified after writing
- on write-verification failure, Tiger attempts to restore the pre-confirm Weekly Data snapshot
- Import Review clears automatically only after core confirmation succeeds

### `Clear Import Review`

Manual discard/recovery action.

This is now on the main Tiger Tracker menu because it is part of import workflow recovery. Normal successful confirmation clears Import Review automatically, so this action is mainly for deliberately discarding an abandoned or incorrect staged import.

---

## Tiger Tracker > Discord

These are Google Sheets webhook posting tools. They overlap with some bot slash commands but remain available as optional/legacy manual posting paths.

### `Post Culvert Leaderboard`

Posts the newest Culvert leaderboard to a saved Discord webhook destination.

### `Post Culvert Leaderboard • choose week`

Prompts for a Tiger week, then posts the leaderboard.

### `Post Guild Weekly Summary`

Posts the newest weekly guild summary through a saved webhook.

### `Post Guild Weekly Summary • choose week`

Prompts for a Tiger week before posting the summary.

### `Post Weekly Milestones`

Posts newest weekly milestone statistics.

### `Post Weekly Milestones • choose week`

Prompts for a week before posting milestone statistics.

### `Set / update Bot API secret`

Stores/updates the Apps Script API secret used by the Worker. The matching Cloudflare secret is `TIGER_API_SECRET`.

### `Add / update destination`

Adds or updates a named Discord webhook destination.

### `Remove destination`

Removes a saved webhook destination.

### `View destinations`

Shows saved destination names without exposing webhook URLs.

---

## Tiger Tracker > Review / Maintenance

### `Re-run name matching`

Re-runs the shared player resolver across Import Review.

Matching can use current IGN, genuine aliases, normalised names, learned OCR Aliases, unique truncated matches, and unique fuzzy matches.

### `Resolve selected row to chosen player`

Resolves the selected Import Review row to the chosen/entered player and can teach the OCR Alias system when appropriate.

### `Add selected row as new player (guided)`

Guided fallback that asks whether the new player should be Active or historical/former.

This overlaps with the two main-menu Add selected actions and is retained as an optional helper.

### `Preview roster changes`

Shows proposed current-roster reconciliation before confirmation when the staged import is eligible.

### `Set up roster tracking field`

Ensures the Players sheet has the required Last Seen field/format for roster reconciliation.

### `Check setup`

Checks required sheets and important headers, including:

- OCR Aliases
- Culvert Role Plans
- Culvert Exemptions
- Discord Import Sessions

### `Set up Discord player fields`

Ensures Discord Username and Discord User ID fields exist in Players.

### `Set up OCR alias learning`

Creates/repairs the hidden OCR Aliases sheet.

### `Set up Culvert role plans`

Creates/repairs the hidden Culvert Role Plans sheet used for short-lived Apply/Cancel plans.

### `Set up Culvert exemptions`

Creates/repairs the Culvert Exemptions sheet used for approved vacations.

### `Set up Discord import sessions`

Creates/repairs the hidden Discord Import Sessions sheet.

### `Rebuild Start Here`

Refreshes the in-sheet workflow guide. The current production guide documents the Discord-first import flow and keeps the screenshot-to-CSV process as a manual fallback.

### `Rebuild Dashboard`

Rebuilds the admin Dashboard from confirmed Weekly Data and current Players state.

### `Rebuild Culvert History`

Rebuilds the guild-wide Culvert history matrix.

### `Rebuild Data Audit`

Re-runs data-quality checks against confirmed data.

### `Rebuild Member Profile`

Rebuilds the Member Profile sheet layout/data while preserving the profile workflow.

### `Refresh profile chart scale`

Refreshes the Member Profile chart scale when needed.
