# WTFPC user guide

This guide is for the WTFPC version named in its Git tag. If a screen differs from this page, use the guide under
the installed release tag rather than `main`.

## What WTFPC does

WTFPC explains local Windows performance evidence. It can collect a point-in-time diagnosis, retain opt-in
background history, and record an explicit Game Mode session. It does not tune Windows, terminate applications,
clear caches, upload diagnostics, run a cloud service, or install updates in the background.

WTFPC separates three ideas that are easy to confuse:

- a **finding** is an observation supported by the evidence shown;
- **severity** estimates impact;
- **confidence** estimates evidential certainty.

A limitation or unavailable collector is not a healthy result and is never silently converted to zero.

## Start with Overview

Overview is passive. Opening it reads current local state but does not start a diagnosis, request elevation,
contact the network, or turn on recording.

Choose **Diagnose now** for a cancelable, non-elevated memory, CPU, disk, and network snapshot. Review the named
limitations before relying on a conclusion. Select a finding in Diagnose to see its evidence and explanation.

## Optional continuous recording

The Agent page controls a current-user scheduled task that retains historical evidence in
`%LOCALAPPDATA%\wtfpc\wtfpc.db`.

- **Install agent** creates the task but does not pretend it is already recording.
- **Start recording** starts the installed task.
- **Stop gracefully** asks the agent to checkpoint and exit; there is no hard-kill fallback.
- **Uninstall task** removes the task and preserves collected history.

If the task target, live process, and database disagree, WTFPC shows that contradiction instead of guessing.

## Game Mode

Game Mode is an explicit session over the existing local recorder. Starting it names any elevation that may be
needed for frame evidence. Stopping waits for evidence to drain and checkpoint. While a session is active, closing
the main window offers to stop, keep recording behind a temporary tray icon, or cancel. WTFPC has no idle tray
process.

Recorded sessions name coverage and omissions. Partial frame or GPU evidence remains partial.
Use `wtfpc game what-changed <session>` to apply the existing factual change timeline to one recorded
session. `--since` may narrow that interval, never widen it beyond the session. Temporal overlap remains
evidence, not proof of cause.

## Local share cards

After selecting a supported finding, inspecting a Game Mode session with usable frame evidence, or loading a
completed Storage ownership snapshot, choose **Share this finding/result**. WTFPC first shows the exact generated
PNG and equivalent text. Nothing has been copied, saved, uploaded, or posted at that point.

Choose **Copy image**, **Save PNG**, or **Copy text** only after reviewing both previews. Closing the preview has no
side effect. Cards contain bounded aggregates and explicit coverage/limitations; they never contain a raw report,
session ID, process ID, username, machine name, endpoint, command line, or file/folder path. An unsafe label,
unknown masking state, unavailable frame result, or layout ambiguity refuses sharing instead of hiding or cropping
the questionable value. Card generation is local and does not open a browser or refresh evidence.

## Ask AI and privacy

Ask AI builds a Markdown report locally. Preview it before disclosure.

- Full diagnostic context is the default because paths and identities can matter to diagnosis.
- **Reduce sensitive details** aliases or removes selected paths, accounts, stable identities, command arguments,
  and endpoints.
- Credential-shaped values are masked in both modes on a best-effort basis.
- Advanced unmasked export must first be made available in Settings, then requires a fresh blocking warning for
  every Copy, Save, or provider action.

Opening ChatGPT or Claude is an explicit third-party disclosure. WTFPC copies the report first and opens only a
compiled HTTPS provider destination after consent. It has no provider API, response ingestion, automatic upload,
or report history. Copy and Save remain local actions.

## Privileged helper verification

The Diagnose page may offer **Verify elevated frame-capture helper**. This is an action-specific identity and
integrity ping, not a generic elevated mode. Official builds verify the installed helper's trusted publisher
before Windows displays UAC. Canceling UAC is safe and suppresses another helper attempt for that app session.

Default startup, diagnosis, reporting, history reads, and agent status do not launch the helper.

## Settings, updates, and local data

Settings contains the exact current-user history and settings paths. It also provides separate confirmations for
deleting collected history and resetting GUI settings/privacy choices; neither action uninstalls WTFPC.

**Check for updates in browser** opens the official Releases page. WTFPC does not download or run an installer.
Before installing an update, verify that the setup has a valid expected publisher and timestamp. Setup preserves
current-user history and uses the stable Program Files path so an installed agent task keeps a stable target.

## Uninstall and complete removal

Normal Windows uninstall stops and unregisters the current-user agent task, removes installed files and
shortcuts, and preserves `%LOCALAPPDATA%\wtfpc`.

The Start menu entry **Uninstall WTFPC and remove my data** starts the complete-removal workflow. It refuses
before UAC if a live agent cannot stop, runs the machine uninstaller, rechecks that no agent owns the data, and
then removes only the exact current user's non-reparse `%LOCALAPPDATA%\wtfpc` directory. Canceling UAC or an
uninstaller failure preserves the data.

## Command-line evidence

The installed CLI is `wtfpc.exe`. Use `wtfpc --help` for the exact grammar shipped in the installed version. The
version-pinned [AI reference](ai-reference.md) lists safe evidence recipes, privilege behavior, vocabulary, and
resource-specific limitations. There are no cleanup, tuning, process-kill, or remote-control commands.

## Getting useful support

Record the installed WTFPC version, Windows version, the exact limitation or error text, and whether the problem
reproduces after **Diagnose now**. Share a reduced Ask AI report first when identities, paths, command lines, or
network endpoints may be sensitive. Never disclose a report you have not reviewed.
