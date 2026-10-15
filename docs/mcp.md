# Installing and configuring the MCP

The macOS MCP launcher is included with **Bob Scheduler 0.1.5 or later**. It uses
the app's bundled runtime and dependencies, so it needs no separate Node install
or source checkout. Bob Shell's own launcher may still require Node.
Settings → MCP and the included agent guides are available in version 0.1.9 or
later; older builds can use the manual setup commands below.

Bob Scheduler is an independent project and is not affiliated with, sponsored
by, or endorsed by International Business Machines Corporation (IBM).
IBM and IBM Bob are trademarks or registered trademarks of IBM. All other
trademarks are the property of their respective owners.

## Install

1. Download the universal macOS app ZIP from
   [GitHub Releases](https://github.com/XReyRobert/bob-scheduler/releases).
2. Extract **Bob Scheduler.app** into **Applications**. macOS 13 or later is required.
3. Open Bob Scheduler and check its Bob path and login in **Settings → Connection**.
4. Open **Settings → MCP**, choose **Standard MCP JSON** or **Bob Shell**, and
   select **Copy client configuration**. Merge the copied entry into your client
   and reload its MCP connections. The copied command follows the app's actual
   location and includes its explicit Bob home override, without API keys.
5. Optionally select **Install MCP command** to create the user-level shortcut.
   Status and Refresh identify missing, installed, outdated or conflicting links.
   Installation applies immediately, independently of the Settings Save button.

The app download contains everything needed for this setup. **Show agent
instructions** reveals the included guide in Finder. The same information is
available without the UI under `Contents/Resources/mcp/AGENTS.md`, with an index
at `Contents/Resources/AGENTS.md` and this guide at
`Contents/Resources/mcp/README.md`.

For manual setup, use this executable as the MCP client's command:

   ```text
   /Applications/Bob Scheduler.app/Contents/Resources/mcp/bob-scheduler-mcp
   ```

No installer is needed when the client uses that full path. For a shorter
user-level command, run the optional installer:

```bash
sh "/Applications/Bob Scheduler.app/Contents/Resources/mcp/install.sh"
"$HOME/.local/bin/bob-scheduler-mcp" --version
"$HOME/.local/bin/bob-scheduler-mcp" --print-config
```

The installer creates a symlink in `~/.local/bin`. It can update a previous
Bob Scheduler MCP symlink and refuses to overwrite unrelated files or links.
It does not edit client configuration or API keys. Use absolute paths when
configuring clients; they may not inherit your terminal's PATH.

The MCP uses the installed app's runtime; keep the app installed.

For a different app location or command directory:

```bash
sh "/Users/yourname/Applications/Bob Scheduler.app/Contents/Resources/mcp/install.sh" --app "/Users/yourname/Applications/Bob Scheduler.app" --bin-dir "/Users/yourname/.local/bin"
```

If the app moves, rerun the installer with its new path and update any client
configuration that points directly inside the old app. To uninstall the optional
command, remove its symlink from `~/.local/bin`; task data stays in Bob's home.

## Client configuration

Running the launcher with `--print-config` produces JSON for its actual absolute
path. Merge its `bob-scheduler` entry into the client's existing `mcpServers`
object, preserving other servers. A default installation can use:

```json
{
  "mcpServers": {
    "bob-scheduler": {
      "command": "/Applications/Bob Scheduler.app/Contents/Resources/mcp/bob-scheduler-mcp",
      "args": []
    }
  }
}
```

### Bob Shell

Generate Bob Shell's configuration with its explicit stdio transport type:

```bash
"/Applications/Bob Scheduler.app/Contents/Resources/mcp/bob-scheduler-mcp" --print-config=bob
```

Merge that entry into Bob Shell's `mcp.json`, either in its user configuration
(`~/.bob/mcp.json` by default) or the workspace's `.bob/mcp.json`. Preserve other
servers and the existing schema declaration. The installed app includes an
example at `Contents/Resources/mcp/config/bob.json`. Restart or reload Bob's MCP
connections after saving.

### Claude Desktop

Open **Settings → Developer → Edit Config** and merge the entry into
`~/Library/Application Support/Claude/claude_desktop_config.json`. Completely
quit and restart Claude Desktop after saving. See the
[official local-server setup guide](https://modelcontextprotocol.io/docs/develop/connect-local-servers).

### Alternate Bob home

The MCP and desktop app must use the same `BOB_HOME`. The default is `~/.bob`.
For a custom home, generate matching configuration with an absolute path:

```bash
BOB_HOME="/absolute/path/to/bob-home" "/Applications/Bob Scheduler.app/Contents/Resources/mcp/bob-scheduler-mcp" --print-config
```

The generated entry includes only that `BOB_HOME` override. It does not copy API
keys or the rest of your environment. Jobs can reuse your saved Bob Shell login;
sign in with `bob` in Terminal first. Optionally configure Bob's API key in the desktop
app's Settings or task environment as usual.

## Verify and use

`--version` should match the installed app version. In your MCP client, list the
available tools: `list`, `view`, `create`, `update`, `pause`, `resume`, `run`, `stop`.

The client launches the server over stdio. Running the launcher without arguments
in a terminal waits for MCP messages; this is normal. `--help`, `--version` and
`--print-config` exit immediately and do not initialize or migrate task storage.

Keep the desktop app open for `run` and `stop`. Other tools can edit task files
while it is closed; the changes apply when the app next opens. The MCP does not
start a second scheduler or execute Bob itself. `run` reports `running`, `queued`
or `skipped`; `started` is true only when a run acquired an execution slot.

If the server is missing, check the absolute command path, install app version
0.1.5 or later, and restart the MCP client. If a task is missing, check that the
app and MCP share the same Bob home. The server writes protocol replies to
stdout and diagnostics to stderr; Claude Desktop's MCP logs are in
`~/Library/Logs/Claude`.

## License

Licensed under the MIT License. The app includes the license at
`Contents/Resources/legal/LICENSE`.
Copyright © 2026 Xavier Rey-Robert.
