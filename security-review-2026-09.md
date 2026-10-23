# Semester Security Review — SEC-REVIEW-2026

**Review reference:** SEC-REVIEW-2026
**Prepared for:** Academic Committee Meeting

---

## 1. Committee meeting details

| Field | Value |
|---|---|
| **Event** | Academic Committee Meeting |
| **Agenda** | Discuss curriculum arrangement and teaching plan for this semester (participants: Department heads) |
| **Date** | Saturday, 24 October 2026 |
| **Time** | 15:00 – 17:00 (Asia/Hong_Kong, UTC+8) |
| **Location** | HKUST Conference Room A |

> **Note on dates:** the calendar contains **no** committee meeting in the first two weeks of September 2026. The only Academic Committee Meeting on record is on 2026-10-24, and the details above are that event's exact details (confirmed by searching the full 2026 calendar).

---

## 2. Advisory facts (high-severity npm advisory)

Selected as the most prominent currently-listed high-severity advisory in the npm ecosystem.

| Field | Value |
|---|---|
| **GHSA ID** | GHSA-h895-cgqg-jf52 |
| **CVE** | CVE-2026-114142 (assigned) |
| **Package** | `toml` (toml-node) — npm ecosystem |
| **Affected versions** | <= 5.0.0 |
| **First patched version** | 5.0.1 |
| **Severity** | **High** (CVSS 8.8 per NVD) |
| **Weakness** | CWE-1921 / CWE-1321 — Improperly Controlled Modification of Object Prototype Attributes (Prototype Pollution) |
| **Source repo** | BinaryMuse/toml-node |

**Summary:** Parsing a TOML document can write to `Object.prototype`. The key-path desynchronization described in CVE-2026-63376 is still reachable when spelled with a quote character instead of a dot, and the owned-container check is not applied on the path that matters. A four-line TOML document parsed with `toml.parse()` writes a property onto `Object.prototype`, where it is visible to every object in the process — including objects produced by unrelated libraries.

**Impact:** Any application that parses a TOML document from an untrusted source (user-uploaded config, third-party manifest, API-submitted document) is affected. Prototype pollution of this kind commonly escalates to authentication/authorization bypass, denial of service, and — with a suitable gadget — remote code execution.

**Remediation:** Upgrade `toml` to **5.0.1+**.

**References:**
- Advisory: https://github.com/advisories/GHSA-h895-cgqg-jf52
- NVD: https://nvd.nist.gov/vuln/detail/CVE-2026-114142
- Fix commit: https://github.com/BinaryMuse/toml-node/commit/609c873fffea0a27ebe5b4dd098eed2b3daf3606
- Release: https://github.com/BinaryMuse/toml-node/releases/tag/v5.0.1

---

## 3. Community discussion

**Issue:** [Triage: toml (GHSA-h895-cgqg-jf52) #8](https://github.com/mcpmark-eval-liuhezi/security-audit/issues/8)
**Repository:** `mcpmark-eval-liuhezi/security-audit`
**Search path:** searched GitHub issues for CVE-2026-114142 → this dedicated triage issue for this exact advisory was the most relevant match.

**What people are actually saying:** The triage issue records the advisory facts (`toml` <= 5.0.0 affected, first patched 5.0.1, high severity, CWE-1321 prototype pollution). Its one discussion comment points to a live, open Dependabot PR — [YieldShield/yieldshield-monad#72](https://github.com/YieldShield/yieldshield-monad/pull/72) — which bumps `toml` from 5.0.0 to 5.0.1 and cites the 5.0.1 release changelog listing GHSA-h895-cgqg-jf52 (prototype pollution writing to `Object.prototype`) as the first advisory it addresses. In short: the community's practical response is exactly the expected one — automated dependency bump PRs to the patched version, verified against the upstream release changelog.

---

## 4. Where the data now lives

- **This repository** (`mcpmark-eval-liuhezi/semester-security-review`) — the reviewable copy of these notes, submitted as a pull request for committee review.
- **Gist:** publication of the gist summary was **blocked** by the personal access token's scope (403 "Resource not accessible by personal access token" on gist creation); the PAT does not grant gist access. This repository serves as the published copy.
- **Snowflake warehouse** (`SECURITY_AUDIT.RESEARCH.review_notes`): **could not be created** — the Snowflake account's free trial has ended and all virtual warehouses are suspended, so every warehouse operation (list databases, create database, run queries, append insight) failed at connection time. Nothing was created in Snowflake. To backfill once access is restored: review ref `SEC-REVIEW-2026`, GHSA `GHSA-h895-cgqg-jf52`, CVE `CVE-2026-114142`, package `toml`, severity `high`, issue link `https://github.com/mcpmark-eval-liuhezi/security-audit/issues/8`.

---

## 5. Recommended actions for the committee

1. Audit the Node-based teaching stack for `toml` (toml-node) usage and bump to >= 5.0.1.
2. Treat any parsing of untrusted TOML input as a risk until patched.
3. Restore the Snowflake warehouse account, then backfill `SECURITY_AUDIT.RESEARCH.review_notes` with the SEC-REVIEW-2026 record above.
