# Aethmere · 识宙

> Public distribution repository — **this is not an open-source repository**.

[简体中文](../../README.md) | **English** | [日本語](README.ja.md) | [한국어](README.ko.md) | [ไทย](README.th.md) | [Tiếng Việt](README.vi.md) | [Bahasa Indonesia](README.id.md) | [Bahasa Melayu](README.ms.md) | [Filipino](README.fil.md) | [Español](README.es.md) | [Português](README.pt.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Русский](README.ru.md) | [العربية](README.ar.md)

Aethmere is a memory layer for AI-assisted work that treats **not making things up**
as an engineering requirement, not a slogan. It gives supported AI clients durable,
user-controlled memory with visible answer boundaries: what you explicitly asked it
to remember is answered exactly; what was never recorded, or was withdrawn, is
refused instead of guessed; ordinary questions pass through to your model untouched.

[Website](https://aethmere.com) ·
[Web app](https://app.aethmere.com) ·
[Latest release](https://github.com/kzkz137806/aethmere-os/releases/latest) ·
[Report an issue](https://github.com/kzkz137806/aethmere-os/issues)

## Why Aethmere

Most AI memory systems fail in one of two directions: they hallucinate memories you
never gave them, or they swallow ordinary questions with needless refusals. Aethmere's
governed memory lane is built so that neither direction can hide:

- **Answerable questions must be answered exactly.** Refusing an answerable question
  counts as a failure in our evaluation — accuracy can never be bought with refusals.
- **Unanswerable questions must be refused.** If a value was never recorded, was
  retracted, or is ambiguous, releasing *any* value would be a fabrication. The
  governed lane refuses, deterministically.
- **Ordinary questions must pass through.** A question that merely mentions memory
  words is routed to your model, not swallowed.
- **Writes are confirmed.** A message that looks like a memory command is written
  only after your explicit confirmation; declining keeps it as ordinary chat history.

## Measured results (sealed, bounded evaluation)

**What was measured:** Aethmere's governed memory contract — its explicit command
grammar and eight query task families — end to end through the real ingestion and
release services. Governed answers are produced by deterministic services, **not by
a large language model improvising**, so the numbers below do not depend on which
provider model you bring.

**How it was measured:** the candidate system was frozen by hash first, and only
then was a pre-committed random seed drawn; cases were generated deterministically,
every answer was scored by a machine oracle fixed at generation time, and all
receipts were kept. Scoring demands exact answers on answerable questions, refusal
on unanswerable ones, and pass-through on ordinary ones — each direction fails
separately, so accuracy can never be gained through refusals.

**What it was compared against:** "before" = the same conversations given directly
to a local qwen2.5:7b (Ollama, temperature 0, no governance); "after" = the
governed memory lane. Baseline scoring is deliberately generous (a reply containing
the correct value counts as correct, Chinese numeral forms included), so the cure
numbers are conservative. The free-form capture lane's proposer is the same local
7B, with zero egress of your original text.

| Task family | Before (7B, ungoverned) | After (governed lane) |
|---|---|---|
| Direct recall | 41 / 300 (13.7%) | **300 / 300** |
| Sets and counting | 98 / 300 (32.7%) | **300 / 300** |
| Time-scoped recall | 63 / 300 (21.0%) | **300 / 300** |
| Updates and conflicts | 41 / 300 (13.7%) | **300 / 300** |
| Multi-hop joins | 65 / 300 (21.7%) | **300 / 300** |
| False-memory pressure | 45 / 300 (15.0%) | **300 / 300** |
| Open key–value notes | 34 / 300 (11.3%) | **300 / 300** |
| Boundary pressure * | 213 / 300 (71.0%) | **300 / 300** |
| **Total** | **600 / 2,400 (25.0%)** | **2,400 / 2,400 (100%, 95% one-sided lower bound ≥ 99.87%)** |

\* Ordinary questions in the boundary family are automatically credited to the
baseline (the model is supposed to answer them), which is why its baseline share
is higher.

The eight task families cover direct recall, sets and counting, time-scoped recall,
updates and conflicts, multi-hop joins, false-memory pressure (where every released
value would be a fabrication), open key–value notes, and boundary pressure (narrative
sentences that must not be ingested, and ordinary questions that must not be
swallowed). Cure accounting: all 1,800 clusters the baseline fabricated or erred on
were **repaired** by the governed lane, with **zero regressions** on the 600 the
baseline got right — bounded cure 100% (95% one-sided lower bound ≥ 99.83%).

**Scope, stated plainly:** these are bounded results on Aethmere's governed memory
contract — its explicit command grammar and query families — measured end to end
through the real ingestion and release services. They are not an open-world claim,
not a whole-product accuracy claim, and not a claim about your model's general
answers. Outside the governed contract, your model answers as usual and normal
model limitations apply.

## What Aethmere does

**Governed memory (the core)**

- Explicit memory commands with exact, auditable semantics: record, update, retract,
  locate, and open key–value notes; multi-value sets; time-scoped recall.
- Every memory is auditable and traceable back to your own words; retracted values
  never surface again through any query.
- Confirm-before-write: new memory commands require your explicit confirmation
  in the product before anything is stored.
- Natural sentences can become memories too: before anything is stored the system
  independently checks it and only accepts content that matches your original
  wording — with zero egress of your original text.

**Skills hub and knowledge base**

- Server-side skills hub: available the moment you log in — a growing library of
  domain capability cards is routed to your question automatically, with no manual
  wiring.
- Personal knowledge base: your uploaded documents become account-isolated,
  searchable private corpus, recalled on demand at answer time.
- Personal cloud-memory recall: across sessions and devices, injecting only
  bounded, relevant fragments for the question at hand.

**Personal cloud memory**

- Account-isolated cloud space (roughly 100M estimated tokens across up to 200
  conversations) with cross-device restore; per-device upload switches; answers
  inject only bounded, relevant history — never the whole archive.
- Provider API keys stored as AES-GCM ciphertext bound to your account; ordinary
  APIs only ever see the last four characters.

**Documents and images**

- Document knowledge base: TXT, Markdown, CSV, JSON, HTML, and PDF; text is
  extracted in your browser and only account-isolated retrieval fragments and a
  hybrid vector index are stored — original files are not kept.
- Image OCR: extracted text is inserted with a source prefix and a
  needs-review summary; recognition runs through your configured provider.

**Real-time search**

- Multi-engine real-time web search with recency windows (day / days / week / month),
  automatic query planning and retries, and result caps tuned for answer grounding.
- Cross-language retrieval: Chinese questions are mapped to focused international
  search topics automatically (markets, commodities, currencies and more).
- Live China futures snapshots for supported symbols, fetched at answer time and
  cited as data sources in the reply.

**Everywhere you work**

- Installable mobile/desktop web app (PWA) with streaming answers, code blocks,
  tables, and one-tap message copying.
- Desktop CLI (`aethmere-cli`) with one-time device linking: `aethmere sync`
  mirrors your cloud memory locally; Claude Code, Codex, and other MCP clients can
  use it through `cloud_memory_recall`. Read-only by default; upload requires an
  explicit double opt-in.
- Chat channels: bind Telegram (bot DM) or Discord (`/aethmere ask`, ephemeral
  replies) to your account with one-time codes; unbinding cuts access immediately.
- Server-side skills hub: curated capability cards are routed automatically after
  login — no manual skill wiring.

## Install Aethmere CLI

Requirements: Node.js 22 LTS (`>=22.13.0 <23`).

```bash
npm install -g https://github.com/kzkz137806/aethmere-os/releases/download/v0.7.0/aethmere-cli-0.7.0.tgz
aethmere --version
aethmere connect
aethmere doctor --profile package
```

Expected version:

```text
Aethmere CLI 0.7.0
```

`aethmere connect` creates a user-level connection for supported AI clients. You
do not need to reconnect when you switch project folders. Local use does not
require a web invitation. Cloud login and synchronization are optional, and
desktop upload remains off until you turn it on.

For a step-by-step Chinese guide, visit
[aethmere.com](https://aethmere.com/#install).

## Verify the download

SHA-256 for `aethmere-cli-0.7.0.tgz`:

```text
964903d1f5787e6fb58dfe37a762d29c966971abd20e06a2b22cdcfe9954a2a6
```

PowerShell:

```powershell
Get-FileHash .\aethmere-cli-0.7.0.tgz -Algorithm SHA256
```

macOS/Linux:

```bash
shasum -a 256 aethmere-cli-0.7.0.tgz
```

The CLI also verifies signed update metadata, package size, and SHA-256 before an
update is installed. Updates are never installed without confirmation.

## What is in this repository

This public repository is the official home for:

- release downloads and checksums;
- installation and update instructions;
- public changelogs;
- issue tracking and security reporting.

The proprietary Aethmere core, private knowledge systems, evaluation material,
service implementation, and internal development history are **not included**.

## Product model

Aethmere uses a public-client/private-core model:

- public distribution and integration entry points;
- proprietary hosted core services;
- downloadable consumer client;
- no public disclosure of the core source code.

The contents of this repository and its release assets are proprietary unless a
file explicitly states otherwise. No open-source license is granted. See
[NOTICE.md](../../NOTICE.md).

## Support

Use [GitHub Issues](https://github.com/kzkz137806/aethmere-os/issues) for public
bug reports and feature requests. Do not include passwords, API keys, private
memories, personal data, or confidential project content.

For security issues, follow [SECURITY.md](../../SECURITY.md).
