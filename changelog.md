# Changelog

All notable changes to this workflow are documented here.

Format: `[Version] — YYYY-MM-DD`

---

## [1.0] — 2026-03-27

### Production release

- Freshdesk webhook trigger on `Kapa Test` tag
- Language detection via code node
- English path: Kapa AI query → public reply → private note with conversation link → AI-handled tag
- Non-English path: language notice reply → reassign to human agent queue
- 7 sticky notes covering all workflow sections
- Tested against live Freshdesk environment
- 90% reduction in average ticket resolution time confirmed

---

## [0.9] — 2026-02-14

### Beta

- Core Kapa AI query and Freshdesk reply logic in place
- Language detection added
- Non-English path added
- Tag merge logic added to preserve existing ticket tags

---

## [0.1] — 2026-01-20

### Initial build

- Webhook trigger + basic Kapa AI HTTP request
- Single-path: English only, no language detection
