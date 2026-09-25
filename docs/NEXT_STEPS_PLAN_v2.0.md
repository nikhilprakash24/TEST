# CVIN-SC — Next-Steps Plan v2.0 (programme level)

**2026-09-25 · supersedes the TEST-only scope of v1.0 (v1.0 + v1.1 addendum retained) · grounded in `PROGRAMME_STATUS_REPORT_v1.0.md` and `CROSS_VERIFICATION_TEST_REPORT_v1.0.md`**

**Governing principle:** the critical path to a thesis is **prose + a date**, not more engineering. Engineering work is justified only when it (a) unblocks a chapter's evidence, or (b) removes a defect that would embarrass the thesis at defence (compiler truth, secrets, broken CI, two v0.8.0s). Everything else waits.

---

## Track 0 — Operator-only (you), this week

| ID | Action | Effort | Unblocks |
|---|---|---|---|
| U1 | **Set a provisional submission window D** (D-2/Q5). Even "Draft to supervisors by 2026-11-30" | 10 min | Every dated plan; MEP phases |
| U2 | **Rotate the Infura key** (MetaMask/Infura dashboard); review usage since 2023-11; record location-only in FINDINGS [TS-4] | 15 min | P0 gate; any release tag |
| U3 | **Spoke collision ruling:** OK to delete onboarding branches + tag `asfound/pre-onboarding` from the SSI-DID spoke (and move `TEST_ONBOARDING.md` → a proper hub back-link line)? | 2 min to decide | Hub rules (no key history in spokes) |
| U4 | Create GitHub account `nikhilprakash-cvin2` **or** amend D-R3 to push the hub under `nikhilprakash24` | 15 min | Hub push (handoff step 1) |
| U5 | Rule: **v1.3 = WO-0 recovery**; compiler truth = **pin 0.8.24 + lockfile, re-baseline F8** (recommended) vs restate as 0.8.37 | 5 min | WO-R, G2, Ch4 |
| U6 | Rule: canonical Lineage B tip = spoke `5a62137` (recommended: it contains the hub's `be32c6e` plus July v0.8.0 and today's fixes) → hub re-imports and bumps to **0.9.0** | 5 min | Kills the duplicate 0.8.0 |
| U7 | TEST stream: approve **D2** (restore from `3b1b4935`) + **D4** (V7 toolbox@^2) | 2 min | Sanctioned WO-S1 → [TS-1] |
| U8 | Correct the supervisor-facing status before the next contact (prose is 0/7 sessions, not "drafted") | 30 min | Credibility |
| U9 | Python: approve installing Python 3.12 on this machine (system change), or name a machine | 2 min | Verifying 28 VC / 21 MOBI / W3C / SUMO claims |

## Track A — Engineering (CLI agent; each item gated on the Track 0 ID shown)

| ID | Work | Gate | Acceptance (evidence) |
|---|---|---|---|
| A1 | **Spoke cleanup:** delete onboarding branches/tag from the spoke; leave the fork `nikhilprakash24/TEST` as the onboarding home | U3 | `git ls-remote` shows spoke clean; FINDINGS entry |
| A2 | **Canon compiler fix, in a hub-bound copy:** pin `"solc": "0.8.24"`, commit `package-lock.json`, add a soljson SHA-256 assertion to the subtask override; re-run G0/G1 → new trace with **true 0.8.24** numbers (F8 expected 62,458); log F-XV-1 in hub FINDINGS | U5 | `npm ci --offline` replay exit 0; compile log longVersion = e11b9ed9; 12/12; new trace file (never overwrite the committed one) |
| A3 | **Import canon into hub** as `sandbox/cvin-sandbox-v1.3/` (+ MISSING.md → RECOVERED.md with provenance: zip SHA-256, v1.3 ≠ v1.1 note) | U4, U5 | Hub commit; SHA256SUMS regenerated; WO-0 closed |
| A4 | **Hub/spoke CI repair:** commit lockfiles for `1_blockchain-identity`; fix the spoke matrix globs + `test:erc*` scripts; fix the "147 tests" label; add `.gitattributes` (`* text=auto eol=lf`) to kill the CRLF gas drift and bundle hash hazard | U6 | CI green on a real run; deploy gas identical on Windows/Linux |
| A5 | **Lineage B re-import** at `5a62137` into hub, version 0.9.0; SISTER_REPOS rows corrected (spoke = "active → frozen after import"; add `CVIN-ID/TEST` + fork + `prevWork` as "survey origin / key source") | U6 | Hub diff; SISTER_REPOS updated |
| A6 | **TEST WO-S1 sanctioned run** (as-found failing log is already captured → restore commit → build+test Node 16 → V7 scaffold) → **[TS-1] resolves** | U7 | 259/259 expected; logs to evidence/ |
| A7 | **B↔C adapter (design only first):** `O1b_ERC1056_Adapter` spec wrapping EthereumDIDRegistry into IMinimalSSI (bytes32↔address, created/deactivated state, credential anchor for F5–F8) so the spoke's CVIN registry can face the 12-test suite | after A3 | Design note → WO proposal (canon rule 7: no scope invention without a WO) |
| A8 | **Python verification** of VC/MOBI/W3C/SUMO claims | U9 | pytest counts; W3C %; SUMO p95 |
| A9 | Vendor solc 0.4.24 (checksum) → Build-0/II native compile → [TS-3] port triage with selector-equality check | after A6 | WO-S2 table rows |

## Track B — Thesis writing (the actual critical path)

Sequence per Scaffolding v0.4 / MWP: **B (Ch2) → C (Ch1) → D (Ch3) → E (Ch4) → F (Ch5) → G (assembly)**, ~12–13k new words on top of ~105 Type-A blocks.

| Session | Chapter | Target words | Evidence dependencies now available |
|---|---|---|---|
| **B** | Ch2 Background & Methodology (start from the existing §2.3 draft) | ~6,500 | ✅ spec frozen; methodology v1.0; **§2.5 must be dated before any WO-1 result (Q15, expiring)** |
| C | Ch1 Introduction | ~3,000 | RQ1–RQ3 adoption (Q16) |
| D | Ch3 Standards Inventory | ~10,000 incl. appendix profiles | WO-6 Inventory v3; option count freeze (Q17/Q18) |
| E | Ch4 Evaluation | ~7,000 | O1 numbers (**after A2 re-baseline**); Lineage B as "indicative"; §4.5 Stage 16 already drafted |
| F | Ch5 Discussion + **§5.6 Conclusions (new)** | ~7,900 | Stage 31 design-only (R-1) |
| G | Assembly, front/back matter, UBC format | — | D, committee, G+PS checklist |

**Recommendation:** start **Session B now**, in parallel with Track 0. It depends on nothing that is blocked.

## Track C — Governance hygiene (low effort, high defence value)

- Freeze the option count (one sentence, CS-4: "17 standalone options across five implementation families (O1–O17), plus the Family F composite (O18)") and sweep 14/19 out.
- Resolve the two "Set B" definitions (ISetB = 4 functions in code) and the spec's `CredentialRevoked` clash (→ `CredentialRevokedEvent`, spec v1.1).
- Namespace work-order IDs across streams: `HUB-WO-*`, `SBX-WO-*`, `ONB-WO-S*`.
- Retire forbidden phrasings still live in Memo A v6 (Sept-3): "validates" → "corroborates".
- Log the TEST repo in the corpus index (it's the key's origin and the survey's origin).

## Suggested order for the next 2 sessions

1. **Now (you):** U1, U2, U3, U5, U6, U7 (≈45 min total).
2. **Next CLI session:** A1 → A2 → A3 → A6 (≈1 session), then A4/A5.
3. **Next writing session:** Session B (Ch2), using the frozen spec + dated §2.5.

## Prompt primers (carry forward)

> *Operate as the lead researcher and principal architect on this programme: verify the on-disk state before asserting, sequence by dependency and reversibility, tie every claim to captured evidence, surface deviations and decisions instead of smoothing them, and stop at my gates.*

Add for this programme: *"Treat the hub `cvin-sc` as canonical, the spokes as frozen unless I say otherwise, the Infura key as P0, and prose — not code — as the critical path."*
