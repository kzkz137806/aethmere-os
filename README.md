# Aethmere · 识海

> Public distribution repository — **this is not an open-source repository**.

Aethmere is a continuity layer for AI-assisted work. It helps supported AI clients
carry approved context across tasks, devices, and models while keeping user control
and answer boundaries visible.

[Website](https://aethmere.com) ·
[Web app](https://app.aethmere.com) ·
[Latest release](https://github.com/kzkz137806/aethmere-os/releases/latest) ·
[Report an issue](https://github.com/kzkz137806/aethmere-os/issues)

## What is in this repository

This public repository is the official home for:

- release downloads and checksums;
- installation and update instructions;
- public changelogs;
- issue tracking and security reporting.

The proprietary Aethmere core, private knowledge systems, evaluation material,
service implementation, and internal development history are **not included**.

## Install Aethmere CLI

Requirements: [Node.js 18 or newer](https://nodejs.org/).

```bash
npm install -g https://github.com/kzkz137806/aethmere-os/releases/download/v0.5.0/aethmere-cli-0.5.0.tgz
aethmere --version
aethmere connect
aethmere doctor --profile package
```

Expected version:

```text
Aethmere CLI 0.5.0
```

`aethmere connect` creates a user-level connection for supported AI clients. You
do not need to reconnect whenever you change project folders. Local use does not
require a web invitation. Cloud login and synchronization are optional, and
desktop upload remains off until the user enables it.

For a step-by-step Chinese guide, visit
[aethmere.com](https://aethmere.com/#install).

## Verify the download

SHA-256 for `aethmere-cli-0.5.0.tgz`:

```text
d8cb5ba07ada5f4ec71ba49356088c6fee1064e55ecb96768fd625e2176290d0
```

PowerShell:

```powershell
Get-FileHash .\aethmere-cli-0.5.0.tgz -Algorithm SHA256
```

macOS/Linux:

```bash
shasum -a 256 aethmere-cli-0.5.0.tgz
```

The CLI also verifies signed update metadata, package size, and SHA-256 before an
update is installed. Updates are never installed without confirmation.

## Product model

Aethmere uses a public-client/private-core model:

- public distribution and integration entry points;
- proprietary hosted core services;
- downloadable consumer client;
- no public disclosure of the core source code.

The contents of this repository and its release assets are proprietary unless a
file explicitly states otherwise. No open-source license is granted. See
[NOTICE.md](NOTICE.md).

## Support

Use [GitHub Issues](https://github.com/kzkz137806/aethmere-os/issues) for public
bug reports and feature requests. Do not include passwords, API keys, private
memories, personal data, or confidential project content.

For security issues, follow [SECURITY.md](SECURITY.md).
