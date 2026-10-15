# Bob Scheduler

Bob Scheduler is a desktop app for scheduling IBM Bob Shell 2 headless sessions.

Bob Scheduler is an independent project and is not affiliated with, sponsored
by, or endorsed by International Business Machines Corporation (IBM).
IBM and IBM Bob are trademarks or registered trademarks of IBM. All other
trademarks are the property of their respective owners.

This public repository contains release downloads and installation and usage
instructions. The application source and development history remain private.
GitHub-generated **Source code** ZIP/TAR downloads contain this documentation
only; download the application ZIP attached to the release instead.

**Current release:** [0.1.9 (prerelease)](https://github.com/XReyRobert/bob-scheduler/releases/tag/v0.1.9).
The universal macOS app is Developer ID signed, Apple notarized and stapled.

## Installation

The macOS release requires **macOS 13 or later** and supports both Apple Silicon
and Intel Macs.

1. Download [Bob-Scheduler-0.1.9-universal-macOS.zip](https://github.com/XReyRobert/bob-scheduler/releases/download/v0.1.9/Bob-Scheduler-0.1.9-universal-macOS.zip).
2. Extract **Bob Scheduler.app** and drag it into **Applications**.
3. Install Bob Shell separately. Its npm launcher also requires Node to be installed.
4. Open Bob Scheduler and check the Bob executable path in **Settings**. Leave it
   as `bob` to use PATH, or enter its full path, such as `/opt/homebrew/bin/bob`.

The release notes state the signing and Apple notarization status. The desktop
app includes its own runtime; Node is needed by Bob Shell's launcher, not to
launch Bob Scheduler.

If a run fails with `env: node: No such file or directory`, check that Node is
installed. The app can find Homebrew's usual `/opt/homebrew/bin` and
`/usr/local/bin` locations when opened from Finder. If Node is installed elsewhere,
include its `bin` directory in the task's PATH environment variable. A task's
explicit PATH replaces the inherited value, so include every directory it needs.

## Getting started

1. Sign in to Bob Shell once by running `bob` in Terminal. Open Scheduler **Settings** and check the Bob path. An API key is optional.
2. Choose **New task** and enter a name, prompt and working directory.
3. Choose a schedule and save the task. Use **Once** for a single dated run.
4. Use **Run now** to try it immediately, then open its run history to read the output.

Tasks continue running while the window is hidden in the system tray. Opening
the app again brings its existing window forward.

## Managing tasks

Each task has a three-dot menu with **Pause/Resume**, **Run now**, **Edit**,
**Share** and **Delete**. The menu supports keyboard navigation and Escape.
Pausing prevents future scheduled runs; use **Stop** to stop a running job.

Share saves a paused TOML copy with a fresh task ID. Task environment variables
and the default API key are excluded. To reuse the exported file, place it as
`automation.toml` in a new folder under `${BOB_HOME:-~/.bob}/automations`, then
review the working directory and environment before resuming it.

The filter button opens a **Filter by** menu with independent **Active**,
**Paused** and **Completed** checkmarks. All are selected initially; toggle any
combination without closing the menu. Completed means a paused one-time task
has finished a run started at or after its scheduled time, including failed
runs. Recurring tasks remain Active or Paused after each run. Filters combine
with name search, and selecting no statuses shows an empty list.

## Settings

Settings opens in a modal dialog over the task view. The selected task, search,
filters, task drafts and live output are preserved when it closes. Its sections
cover the Bob connection, storage, execution and startup preferences. Save and
Cancel stay visible while the contents scroll.

**Save** applies preferences and is disabled for unchanged or invalid values.
Close, Escape and backdrop clicks ask before discarding unsaved edits. Saving
locks inputs and dismissal until it finishes.

Use **Browse** to choose the run history folder. Saving copies and merges existing
history into that folder immediately, including history for active runs. The
original files remain available. Maximum concurrent runs accepts whole numbers
from 1 to 16; the default is two.

Without an API key, jobs reuse your saved Bob Shell login. If the login is missing
or expired, sign in through `bob` in Terminal and retry the job. Bob Shell 2.0.6
or later is required for this integration.

If your Terminal already exports an API key, start a browser-login session with
`env -u BOB_API_KEY -u BOBSHELL_API_KEY bob`. This omits those variables for that
command only; your Terminal configuration stays intact.

The optional default API key is stored in the OS keychain and has separate **Save key /
Replace key** and **Remove key** controls. The key is masked by default, and
removing a saved key requires confirmation. Clearing its input does not remove
the stored key. Preferences can still be saved if the keychain is unavailable;
keychain failures do not apply preference drafts. **Retry keychain** reloads its
status without discarding other preferences.

## Execution and history

Run output streams into the app, and history records success, failure or
interruption. **Stop** stops running work or cancels a queued run. Extra tasks
wait in order when the concurrency limit is reached. Deleting a task or pausing
a scheduled task cancels its queued run; reducing the limit lets active runs
finish. A trigger while the same task is already running or queued creates a
skipped history entry.

A task can set an optional timeout in **Advanced**. Blank or zero means no
timeout; the maximum is 1440 minutes. The timer starts when Bob starts, excluding
time spent queued or waiting for the keychain. A timeout stops the run and records
a failure.

Quitting cancels queued work, stops running work and saves final history. Queued
work is not replayed on the next launch.

Output is limited to 100 KB per run, with a visible truncation marker. History
normally retains up to 200 entries per task, preserving active entries until
they finish.

## Advanced usage

### Schedules and timezones

The editor supports five-field cron, one-shot dates and a validated RRULE subset:

- Daily: `FREQ`, `INTERVAL`, `BYHOUR`, `BYMINUTE`.
- Weekly: those fields plus plain `BYDAY` values, such as `MO,FR`.
- Monthly: those fields plus either positive `BYMONTHDAY` values or numbered
  weekdays such as `1MO` / `-1FR`.
- Yearly: those fields plus one `BYMONTH` and optional positive `BYMONTHDAY` values.
- Minutely/hourly RRULEs are supported when their intervals divide 60/24
  respectively; hourly rules can include `BYMINUTE`.

Calendar RRULE intervals range from 1 to 99 and use the task's creation date as a
stable anchor, including across restarts and year boundaries. Weeks start on
Monday. Cron step intervals reset at field boundaries; they are calendar steps
rather than continuous elapsed-time intervals. Cron expressions that cannot be
represented by the editor's simpler controls remain editable as literal cron.

Unsupported RRULE constraints, including `COUNT`, `UNTIL`, `BYSETPOS` and unknown
fields, are rejected. Use **Once** for a single dated run.

Timezone defaults to system local time. For one-shot dates and calendar RRULEs,
nonexistent local times during daylight-saving changes are skipped; the editor
rejects a nonexistent one-shot time. An ambiguous time uses its earlier UTC
occurrence. Recurring cron follows the cron engine's timezone behavior.

### Authentication and Bob options

Jobs without an effective API key use `bob acp` to reuse Bob Shell's stored SSO
login. Bob manages the token and its refresh; Scheduler does not copy it or open
a browser during a scheduled job. A missing/expired login produces a sign-in
message in run history. See [IBM's ACP documentation](https://bob.ibm.com/docs/shell/features/acp).

API-key authentication precedence is a task's `BOB_API_KEY` (or legacy `BOBSHELL_API_KEY`),
then the default Settings key, then inherited environment values. Explicit task
keys and the Settings key are supplied under both names for compatibility.

Supported Bob 2 flags include `--mode`, `--trust`, `--accept-license`,
`--max-turns`, `--max-cost`, `--format`, `--log-level`, `--disable-tool-groups`,
`--disable-mcp`, `--disable-subagents` and `--team-id` for API-key jobs, which use
`bob run`. The legacy sandbox checkbox disables the `edit` and `execute` tool groups.

Login-based jobs support mode, trust/license preferences, log level, Disable MCP,
Disable subagents and the scheduler's timeout. Bob Shell 2.0.6's ACP interface does
not expose per-job cost/turn limits, JSON output, tool group restrictions, Team ID
or extra CLI arguments. Such options produce an error asking for an API key;
they are never silently dropped. Scheduled ACP jobs use Bob's automatic approvals
where its safety rules permit. An operation needing interactive approval fails
with an explanatory message instead of waiting indefinitely.

### Data folders and recovery

Tasks live in `${BOB_HOME:-~/.bob}/automations/<id>/automation.toml`. Settings and
history default to `${BOB_HOME:-~/.bob}/scheduler`. Existing legacy JSON tasks
are imported once. Set `BOB_HOME` before launching to use an alternate home.
An alternate home does not import the system user's legacy task files.

If settings or history are invalid, the app preserves the affected file as
`.corrupt-<id>`, recovers a valid `.bak` backup or uses defaults, and displays a
warning with the preserved file's location.

### Optional MCP integration

The MCP server exposes `list`, `view`, `create`, `update`, `pause`, `resume`, `run`
and `stop` for use by an MCP client. Creating the same name and directory updates
the existing task. The optional `timeoutMinutes` field is accepted by create/update.
The macOS app includes the server, launcher, installer and setup guides. Open
**Settings → MCP** to copy client configuration, install the optional user
command, check its status or reveal the agent instructions. The app download is
sufficient; no separate MCP ZIP, source checkout or Node installation is needed
for this server. See [MCP installation and client configuration](docs/mcp.md).

Execution remains in the desktop app. `run`/`stop` fail when the app is closed;
task file updates apply on its next launch. `run` reports `running`, `queued`, or
`skipped`, and `started` is true only when a run acquired an execution slot.
The server follows history folder changes without needing to restart.

## Downloads and updates

Check [Releases](https://github.com/XReyRobert/bob-scheduler/releases) for updates. Quit Bob Scheduler
before replacing the app in Applications; your tasks and history stay in Bob's home folder. Back up that folder before updating. Version 0.1.9 is a prerelease.

The application ZIP is the only uploaded release asset. It includes the MCP
server, runtime, installer, configuration examples and agent instructions. See
the [MCP setup guide](docs/mcp.md) or [agent setup guide](docs/mcp-agents.md).
Notices, SBOMs and corresponding third-party library sources are included in the
app under **Help → Open Source Licenses**.

To verify version 0.1.9, run
`shasum -a 256 Bob-Scheduler-0.1.9-universal-macOS.zip`. Its SHA-256 is:

```text
f70a0d9f351a58ba58818135825784109932dff23f4dd6472f70746018c6c5b9
```

## License

The distributed application retains its [MIT License](LICENSE). Publishing
these downloads does not publish the application's development source tree.
Copyright (c) 2026 Xavier Rey-Robert.

Third-party components retain their own licenses. Open **Help → Open Source
Licenses** for the included notices, SBOMs and corresponding Electron/FFmpeg
library sources.
See [license and dependency information](docs/licensing.md).
