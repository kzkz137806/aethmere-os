# Changelog

## Unreleased

- Added the public Aethmere hallucination-governance scope and its evidence
  boundary.
- Published the conditions for a fair, same-model cross-vendor comparison
  instead of treating search answer accuracy as a hallucination cure rate.

## 0.7.0 — 2026-07-28

- Added status overview, session handoff, task convergence, and multi-session
  coordination commands.
- Added user-managed private skill lifecycle, team roles, and allowlisted
  automation with explicit approval.
- Moved shared skill and knowledge use to task-scoped remote calls; the client
  does not download or cache the shared capability source.
- Retained account-level cloud memory, personal document knowledge, trusted
  search, explicit upload controls, and signed rollback-capable updates.

See the [0.7.0 release](https://github.com/kzkz137806/aethmere-os/releases/tag/v0.7.0)
for downloads and checksums.

## 0.5.0 — 2026-07-15

- Added a user-level Aethmere connection for supported AI clients.
- Removed the need to reconnect when switching project folders.
- Kept cloud login and synchronization separate from local client connection.
- Kept desktop upload disabled until explicitly enabled by the user.
- Added signed update checks, explicit update confirmation, and rollback support.
- Added package diagnostics through `aethmere doctor --profile package`.

This historical package remains available through the official update channel
for rollback.
