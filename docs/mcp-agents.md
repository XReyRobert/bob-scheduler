# Bob Scheduler MCP setup for agents

Use these instructions when the user asks you to connect an MCP client to Bob
Scheduler. The server and runtime are already included in the macOS application.
Do not install an npm MCP package, download another runtime, or modify the signed
app bundle. Bob Shell is a separate installation and may require Node itself.

## Locate the installed app

When this file is inside an application bundle, its directory is
`Bob Scheduler.app/Contents/Resources/mcp`. Resolve the actual absolute path of
that app, including spaces or a relocated name. Do not assume `/Applications`.
An additional index is at `Contents/Resources/AGENTS.md`; the human guide is
`Contents/Resources/mcp/README.md`.

The optional MCP setup ZIP also includes this guide, but contains no app or
runtime. In that case locate the user's installed app first. A typical location
is `/Applications/Bob Scheduler.app`.

## Generate the client configuration

Set APP to the actual app path in your shell, preserving quotes:

```bash
APP="/Applications/Bob Scheduler.app"
LAUNCHER="$APP/Contents/Resources/mcp/bob-scheduler-mcp"
"$LAUNCHER" --version
"$LAUNCHER" --print-config
```

`--version` must match the installed app version. `--print-config` emits an
`mcpServers` JSON object with the actual absolute launcher path and an empty args
array. A path containing spaces must stay a single command value, not a shell
command string. For Bob Shell use `--print-config=bob`, which adds `type: stdio`.
Settings → MCP can copy either format without running a child server.

Merge only the `bob-scheduler` entry into the client's existing configuration,
using the client's supported file/schema format. Preserve other servers, unknown
fields, comments where supported, and all existing credentials. Do not replace a
whole config file with the generated example. Reload or restart that client's
MCP connections after an authorized change.

## Use the same Bob home

The desktop app and MCP must share the same Bob home. The default is `~/.bob`.
If the app was launched with BOB_HOME, generate the entry with the same absolute
path, or copy it from the app's Settings:

```bash
BOB_HOME="/absolute/path/to/bob-home" "$LAUNCHER" --print-config
```

Generated configuration includes only this BOB_HOME override. Do not copy the
agent's environment, API keys, SSO tokens or keychain values into client config.
No API key is required merely to install/connect this MCP. Scheduled jobs use
the app's normal Bob Shell login or optional configured key.

## Optional user command

The absolute bundled launcher is sufficient. When the user wants the shorter
command, use Settings → MCP → Install MCP command, or:

```bash
sh "$APP/Contents/Resources/mcp/install.sh" --app "$APP"
"$HOME/.local/bin/bob-scheduler-mcp" --version
```

This creates a managed symlink under `~/.local/bin`, without sudo or PATH edits.
It can update an earlier Bob Scheduler link after the app moves, and refuses
unrelated files/links. If there is a conflict, report it; do not delete or replace
the unrelated destination. It does not modify client config or credentials.
Rerun it if the app moves; configuration pointing directly into the old app must
also be updated. Uninstalling the shortcut means removing only that managed
symlink, not the application, client configuration or task data.

## Verify without executing a task

The client launches LAUNCHER over stdio with no arguments. A terminal invocation
with no arguments waits for protocol messages; it is not an interactive CLI.
Use the client's connection test, MCP initialize handshake and `tools/list` to
verify connection. The server supports MCP protocol `2024-11-05`; its initialize
reply reports the installed app version. Tools are `list`, `view`, `create`,
`update`, `pause`, `resume`, `run`, `stop`. Keep stdout for JSON-RPC messages;
diagnostics belong on stderr. CLI help/version/config modes exit without
initializing task storage.

Verification does not require creating a task or calling `run`. Only change or
execute tasks when requested by the user. Keep the desktop app open for `run`
and `stop`; the MCP routes execution to it and never starts a second scheduler.
Other tools can edit task files while the app is closed, and changes apply when
it next opens. If tasks are missing, check BOB_HOME before changing anything.

Copyright © 2026 Xavier Rey-Robert. MIT licensed. This is an independent project,
not affiliated with, sponsored by or endorsed by IBM.
