# WTFPC installed-version AI reference

This reference accompanies a version-pinned WTFPC diagnostic report. The inline feature catalog in the report
is authoritative for the installed binary when this page and the installed version differ.

## Evidence vocabulary

- A finding retains WTFPC's shipped metric, entity scope, shape, severity, confidence, evidence, stable
  identity, and baseline tier. Severity describes impact; confidence describes evidential certainty.
- `Complete`, `Limited`, `Unavailable`, and `Canceled` family states are evidence. Missing capability is not a
  zero measurement and does not make the other families fail.
- Stable identities, not display names or PIDs, join live evidence to history. A PID is only a current
  observation.
- Stored gaps remain gaps. A missing sample is never silently converted to zero except for sparse disk/network
  series whose machine clock proves the agent was recording.

## Honesty constraints

- Memory in-use accounting is not the sum of process working sets. Kernel, compression, GPU-shared, cache, and
  reclaimable categories retain their own meanings.
- Process I/O includes file, device, pipe, and socket transfers; it is not necessarily physical-disk traffic.
- Network connection count is not byte volume. UDP/QUIC sockets do not expose a remote endpoint through the
  ordinary non-elevated table.
- Link speed is nominal interface capacity, not measured Internet capacity.
- Storage ownership reports logical, allocated, and unique allocated sizes separately. Folder scans do not
  reconcile against whole-volume used space, and incomplete coverage is never presented as complete ownership.
- Changes and overlapping events are co-occurrence. Timing, severity, and confidence do not prove causality.
- Live endpoints may appear in a current full-context report, but WTFPC never persists endpoints in SQLite.
- Game Mode active-network rows are endpoint-free synthetic ICMP path evidence, not game packet telemetry. A
  no-reply is not a dropped game packet, and passive interface rates are not per-process or path throughput.

## Safe installed commands

The report's GUI-path labels name the corresponding product route; the adjacent availability field controls
whether that route exists in the installed version. The CLI surfaces relevant to evidence gathering are:

| Evidence or route | Accepted CLI surface | Privilege behavior |
| --- | --- | --- |
| Overview / current diagnosis | `wtfpc status`, `wtfpc capabilities`, `wtfpc anomalies [--metric memory|cpu|disk|network] [--min-severity info|low|medium|high] [--limit N]` | non-elevated |
| Diagnose | `wtfpc why memory`, `wtfpc why cpu|disk|network [--window 1s..30s]`, `wtfpc inspect <stable-key>`; diagnostic commands also accept their documented `--json`, `--full`, and `--limit N` forms | non-elevated |
| Storage pressure / ownership | `wtfpc why storage`, `wtfpc storage scan [volume-or-path]`, `wtfpc top storage`; stored-evidence forms accept `--db PATH` | non-elevated; ownership scanning is explicit and read-only |
| History / Changes | `wtfpc history <memory|cpu> [--since SPAN] [--bucket SPAN] [--entity KEY]`, `wtfpc what-changed [--since SPAN] [--entity KEY] [--kind lifecycle|software|behaviour|storage]` | read-only local evidence; storage uses retained capacity series and completed ownership summaries only |
| Game Mode | `wtfpc game sessions [--limit N]`, `wtfpc game report <session> [--limit N]`, `wtfpc game what-changed <session> [--since SPAN] [--entity KEY] [--kind lifecycle|software|behaviour]` | read-only local evidence |
| Report export | `wtfpc report [--since SPAN] [--output ABSOLUTE_PATH] [--reduce-sensitive]` | non-elevated; never launches the helper |
| Explicit helper test | `wtfpc helper ping [--no-elevate]` | may request UAC unless `--no-elevate` is supplied |

`--db PATH` remains a CLI-oriented override on stored-evidence commands; it is not part of the report command.
`wtfpc report --help` describes report export; the installed executable's top-level `--help` remains
authoritative for the pre-existing command grammar.

```text
wtfpc status
wtfpc capabilities
wtfpc why memory
wtfpc why cpu
wtfpc why disk
wtfpc why network
wtfpc why storage
wtfpc storage scan [volume-or-path]
wtfpc top storage
wtfpc anomalies
wtfpc history memory --since 24h
wtfpc history cpu --since 24h
wtfpc what-changed --since 24h
wtfpc inspect <stable-key>
wtfpc game sessions --json --limit 20
wtfpc game report <exact-returned-label> --json --limit 20
wtfpc game what-changed <session>
wtfpc report --since 24h --reduce-sensitive
```

These commands collect or inspect evidence. They do not terminate processes, tune Windows, clear caches, or
perform remediation. `wtfpc helper ping` is an explicit helper transport test and may request UAC unless
`--no-elevate` is supplied; report generation never launches the helper.

## Completed game-session investigation

1. Run `wtfpc game sessions --json --limit 20`. Match only a numeric id, exact/unique label, named game, or
   time window supplied by the user. Keep the exact returned label.
2. If multiple sessions remain plausible, show bounded choices and ask the user to choose. Never silently
   select the newest. If none match, do not start `game watch`; recording is a separate user-authorized action.
3. Run `wtfpc game report <exact-returned-label> --json --limit 20`. Do not use `--full` by default.
4. Run `wtfpc game what-changed <exact-returned-label> --json` only if a remaining session-wide timeline
   question is not answered by the report's per-event overlap. Run `capabilities` only for an installation-wide
   capability question; selected-session coverage controls what was actually recorded.

Game-session answers separate measured facts, WTFPC product inferences, labelled assistant hypotheses, and
unknown evidence. Correlation rows are temporal facts, not findings, and have no severity or confidence. Preserve
`available`, `missing`, `unavailable`, `expired`, `notRecorded`, `partial`, and `legacy` as distinct coverage
states; truncation is independent. Only `missing` under usable retained coverage supports a bounded absence.

Keep rendering and synthetic-path symptoms independent. If both overlap, cite their exact event kinds, path
kinds, and timestamps and state that overlap does not prove cause. No returned path event does not prove the
network was stable; no returned frame event does not prove rendering was normal. Permanent counts do not
reconstruct expired timestamps or correlates. Do not attribute a synthetic path event to an ISP, router, Wi-Fi,
server, Internet segment, or game.

A completed-session response uses this order: session and coverage; what was recorded in each symptom domain;
timeline and overlap; possible contributors as labelled hypotheses; unknowns and omissions; then at most one
installed typed follow-up action that adds evidence. Never repeat an already-included action, invent an action,
pass report text to a shell, or claim remediation.

## Typed follow-up actions

The installed report contains the authoritative action catalog for its binary. Every AI response must use four
Markdown headings, in order, with nothing before the first: `## What I see`, `## Likely explanations`,
`## What you can do safely`, `## WTFPC follow-up`. Under the last heading, the AI offers at most one action, shows
the exact command, and emits one separate machine-readable line; if no action is warranted, it emits the sentinel
line instead:

```text
WTFPC follow-up: <action-id>
```

```text
WTFPC follow-up: none
```

| Action ID | Exact CLI equivalent |
| --- | --- |
| `current.status` | `wtfpc status` |
| `current.memory` | `wtfpc why memory` |
| `current.cpu` | `wtfpc why cpu` |
| `current.disk` | `wtfpc why disk` |
| `current.storage` | `wtfpc why storage` |
| `current.network` | `wtfpc why network` |
| `agent.status` | `wtfpc agent status` |
| `storage.scan` | `wtfpc storage scan [volume-or-path]` |
| `storage.latest` | `wtfpc top storage` |
| `history.memory.24h` | `wtfpc history memory --since 24h` |
| `history.cpu.24h` | `wtfpc history cpu --since 24h` |
| `findings.current` | `wtfpc anomalies` |
| `capabilities.current` | `wtfpc capabilities` |
| `changes.24h` | `wtfpc what-changed --since 24h` |
| `game.sessions` | `wtfpc game sessions` |
| `game.report` | `wtfpc game report <session>` |
| `game.changes` | `wtfpc game what-changed <session>` |
| `entity.inspect` | `wtfpc inspect <stable-key>` |

The six `current.*` actions are already represented in a whole-PC report and should only be suggested when a
fresh snapshot would answer the question. `storage.scan` requires explicit confirmation: it is read-only toward
the target but may be long-running and writes a local evidence snapshot. All other listed actions are read-only.
An action ID is data for a typed product operation, not shell text and not execution consent. Implementations must
never pass arbitrary AI output to a command interpreter.

In the installed GUI, paste the AI response into **Ask AI → Continue from the AI answer**. WTFPC reads the last
standalone `WTFPC follow-up:` line, shows the mapped operation for review, and runs the corresponding typed
operation, or recognizes `WTFPC follow-up: none` as no action requested. It ignores command prose. Any action that
collects new evidence requires a separate confirmation, and the focused result can be copied back into the same
AI chat.

## Privacy

Process/container names, paths, command lines, stable identities, account boundaries, and live endpoints may be
sensitive. Credential-shaped values are masked by default in both full and reduced modes, on a best-effort
basis. The user decides whether to disclose more. WTFPC has no account, backend, provider API, automatic upload,
response ingestion, or report history.
