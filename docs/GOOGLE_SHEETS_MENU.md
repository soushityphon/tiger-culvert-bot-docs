# Google Sheets Menu Guide

Spreadsheet access should be limited to trusted admins.

This guide documents the day-to-day Tiger Tracker menu actions. Private bot configuration and deployment settings are intentionally not covered here.

# Tiger Tracker

## `Import weekly CSV`

Manual fallback importer for weekly screenshot-derived CSV data.

The Discord screenshot importer is the normal workflow.

## `Add selected as active player`

From an Import Review row, creates a new player record marked Active and resolves the selected review row.

Use this for a genuinely new current member.

## `Add selected as former player`

From an Import Review row, creates a historical/former player record without making that player Active.

Use this for older historical imports.

## `Confirm week into Weekly Data`

Validates Import Review and writes the selected week into Weekly Data.

Important safety behaviour includes:

- every staged row must be resolved before confirmation
- duplicate Player IDs are blocked
- required numeric values must be valid
- historical imports show the exact target date before writing
- replacing an existing week requires confirmation
- eligible current imports can show roster changes before approval
- Tiger verifies the target Weekly Data week after writing
- if core write verification fails, Import Review is kept for recovery
- Import Review clears automatically only after successful core confirmation

## `Clear Import Review`

Manual discard/recovery action.

Normal successful confirmation clears Import Review automatically.

Use this only when intentionally discarding an incorrect, abandoned, or unwanted staged import.

---

# Tiger Tracker > Discord

These are optional Google Sheets posting tools that overlap with some Discord bot commands.

## `Post Culvert Leaderboard`

Posts the newest Culvert leaderboard through a saved Discord destination.

## `Post Culvert Leaderboard • choose week`

Prompts for a Tiger week, then posts the leaderboard.

## `Post Guild Weekly Summary`

Posts the newest weekly guild summary.

## `Post Guild Weekly Summary • choose week`

Prompts for a week before posting the summary.

## `Post Weekly Milestones`

Posts the newest weekly milestone statistics.

## `Post Weekly Milestones • choose week`

Prompts for a week before posting milestone statistics.

Other items in this submenu are maintenance/configuration tools and are intentionally not documented in this public admin guide.

---

# Tiger Tracker > Review / Maintenance

## `Re-run name matching`

Re-runs Tiger's player matching across Import Review.

Use this after fixing player data or when staged rows should be matched again.

## `Resolve selected row to chosen player`

Resolves the selected Import Review row to an existing player.

## `Add selected row as new player (guided)`

Guided fallback for adding a new current or historical/former player.

The two main-menu Active/Former actions are usually clearer.

## `Preview roster changes`

Shows proposed current-roster changes before confirmation when the staged import is eligible.

## `Check setup`

Checks that the tracker has the sheets and fields needed for normal operation.

## `Rebuild Start Here`

Refreshes the in-sheet workflow guide.

## `Rebuild Dashboard`

Rebuilds the admin Dashboard from confirmed data.

## `Rebuild Culvert History`

Rebuilds the guild-wide Culvert history view.

## `Rebuild Data Audit`

Re-runs data-quality checks against confirmed data.

## `Rebuild Member Profile`

Rebuilds the Member Profile view.

## `Refresh profile chart scale`

Refreshes the Member Profile chart scale when needed.

Other setup items in this submenu create or repair machine-managed tracker sheets and fields. They are normally only needed during initial setup or troubleshooting.
