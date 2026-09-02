# WTFPC

**What the fuck is my PC doing?**

> ## ⚠️ Internal beta — testing only
>
> This repository hosts pre-release builds of WTFPC for internal business
> testing, ahead of a signed public release. Builds published here:
>
> - are **not** signed with the production Suma Sapo Enterprises, Inc. code-signing
>   identity — they carry a development/beta signature, or none;
> - are **not** the final release candidate — features, output, and CLI
>   surface may still change;
> - are for the invited test group only. Please don't redistribute the
>   download link or the installer outside that group.
>
> None of that affects the license below, which applies to every build
> regardless of signing or release status.

## What it does

Task Manager gives you ~400 processes and a "Memory" column that doesn't
reconcile with itself. WTFPC does the correlation for you and shows its
work — where memory, CPU, disk, and network activity actually went, broken
down by real-world groupings (a browser's tabs/GPU/extensions, WSL, a
service host's children) instead of a raw process list.

```
$ wtfpc why memory

In use 36.7 GB of 63.2 GB (58%)

  VS Code                        6.87 GB    32 processes
  Firefox                        4.12 GB    17 processes
  Memory Compression              4.59 GB
  Docker Desktop (vmmemWSL)       1.81 GB
  ...
```

## Download

Grab the latest installer from the [Releases page](../../releases). Each
release lists what changed and what's still rough. Requires Windows 10/11,
x64.

## License

WTFPC is licensed under the [PolyForm Shield License 1.0.0](LICENSE):
free to use for any purpose, including at work and inside a business,
with one exception — you may not use it to build a product that competes
with WTFPC or with other products Suma Sapo Enterprises, Inc. provides
using it. Source-available, not OSI "open source"; that trade is
deliberate, not an oversight.
