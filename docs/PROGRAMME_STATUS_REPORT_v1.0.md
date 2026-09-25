# CVIN-SC Thesis Programme — "How It's Going" Status Report

**v1.0 · 2026-09-25 (UTC) · Opus 5.5 team pass (2 researchers, 4 engineers, 1 synthesis architect)**
**Scope:** the whole thesis programme, not just the TEST onboarding. Every verified number is backed by a file in `evidence/XV_*` (63 files, SHA-256 manifested). "Doc-claimed" means the corpus says it and we did not re-measure it.

---

## 0. The one-paragraph answer

**The engineering is in better shape than the documents say. The thesis prose is in much worse shape than the supervisors have been told. And the whole programme is waiting on one date that doesn't exist yet.** Every code claim we could check reproduced exactly on this machine, across three independent code lineages:
- canon sandbox O1: 12/12 tests, gas delta 0 on all 13 transactions;
- hub Lineage B: 201/201 tests, with the 63/63 gas matrix and 43/43 security matrix identical;
- spoke: 47/47 tests, every gas figure identical;
- TEST Build-III (restore preview): 259/259 tests.

The hub's "blocking" WO-0 ("the sandbox is missing") is effectively solved: the sandbox is `cvin-sandbox-v1.3`, sitting in Downloads. Against that, thesis prose stands at **0 of 7 writing sessions**: about 2,500 words exist against a ~46,600-word architecture. There is **no submission or defence date (D-2/Q5)** and no named committee (Q6). The canonical hub remote also doesn't exist yet.

---

## 1. Overall progress (approximate)

```
OVERALL THESIS COMPLETION   [███████░░░░░░░░░░░░░]  ~35%   (weighted: prose 40%, empirical 25%, spec 15%, infra 10%, institutional 10%)
```

| Workstream | Doc-claimed (STATUS 09-23 / Board 08-18) | Team-assessed today | Bar | Basis |
|---|---|---|---|---|
| Specification & interface (IMinimalSSI F1–F12) | 100% | **100%** | `[████████████████████]` | 12 signatures byte-identical across canon, spoke copy and hub spec (verified). Small defects: the `CredentialRevoked` error/event clash in the hub spec; two definitions of "Set B" (4 functions vs 12) |
| Planning / scaffolding / verification of the corpus | 100% / ~80% | **~85%** | `[█████████████████░░░]` | Scaffolding v0.4, Registry v12, 5 verification passes. The Suite Verification Report's 5 BLOCKING findings have been claimed fixed but were not re-verified |
| Empirical — Lineage A (IMinimalSSI canon) | ~60% (09-23) / ~90% (08-18) | **~30%** | `[██████░░░░░░░░░░░░░░]` | 1 of 9 EVM options implemented (O1); gates G0/G1 reproduced by the agent (not yet operator-executed); **G2–G6 not run**; S1 of S1–S6 only; WO-1…9 open |
| Empirical — Lineage B (9-standard implementation) | (in the ~60%) | **~75%** | `[███████████████░░░░░]` | 201/201 + 63/63 gas + 43/43 security reproduced. Python layers (28 VC, 21 MOBI VID, 12 use cases, W3C 93.2%/89.6%, SUMO 0.27 ms) **unverified** (no Python on the host). Not IMinimalSSI-comparable (viaIR on; address-typed DIDs) |
| Repository consolidation | ~70% | **~65%** | `[█████████████░░░░░░░]` | Hub bundle intact (241/241 SHA). Blocked: account `nikhilprakash-cvin2` = 404; hub believes WO-0 is unsolved; spoke no longer frozen; version 0.8.0 exists twice; lockfiles missing so CI is broken in hub and spoke |
| Correction propagation (K-1…K-43) | ~10% | **~10%** | `[██░░░░░░░░░░░░░░░░░░]` | Applied to 0 of 168 blocks (Manual v2.0) |
| Analytical hardening (G-A1…G-H3) | ~85% produced / 0% placed | **~40%** effective | `[████████░░░░░░░░░░░░]` | Produced but not placed into chapters |
| **Thesis prose** | 0 / 7 sessions | **~5%** | `[█░░░░░░░░░░░░░░░░░░░]` | Only prose: Ch2 §2.3 draft (~2,500 words). Sessions B–G not started |
| Security thread (G-C1) | ~20% | **~20%** | `[████░░░░░░░░░░░░░░░░]` | Iteration 1 closed; Iterations 2–4 blocked on S1 issuance / Q10 |
| Literature & positioning | ~65% | **~65%** | `[█████████████░░░░░░░]` | RW audit v4 run 06-22: C1 "defensible but narrowly bounded"; G-D1 search "partially executed" |
| Institutional (UBC G+PS) | ~10% | **~5%** | `[█░░░░░░░░░░░░░░░░░░░]` | No D, no committee, supervision contradicted between documents; all 17 G+PS items are "confirm" tokens |
| TEST-repo onboarding (this stream) | — | **~45%** | `[█████████░░░░░░░░░░░]` | WO-S0 ✅, P0 ✅, P1 ✅, WO-S1 *preview* ✅ (259/259); sanctioned WO-S1 run, WO-S2…S4 open |

---

## 2. Phase view (Master Execution Plan P0–P6, as of today)

| Phase | Content | State |
|---|---|---|
| **P0 Secure & Freeze** | Push sandbox, rotate key, tag baseline, freeze counts | 🟨 **partial.** Baseline tags exist in the bundle, and the sandbox has been located (v1.3). The key is **not rotated** (public since 2023-11, now also copied into the spoke). Option count not frozen (14/17/18/19 still circulate) |
| **P1 Foundations** | Spec, interface, methodology, scaffolding | ✅ done (spec frozen; methodology v1.0; scaffolding v0.4) |
| **P2 Empirical (critical path per MEP)** | WO-1…3 porting, WO-5 coverage matrix, gates G2–G5 | 🟥 **not started.** Only O1 and S1 exist. Scaffolding v0.4 re-labels porting as "optional WO-1-lite", which conflicts with CLAUDE.md, where WO-0…5 is the minimum core |
| **P3 Analysis & Inventory** | Inventory v3 (WO-6), latency/ZKP as analysis (R-1) | 🟨 specified, not written |
| **P4 Generalisation** | Stage 31 agent wrapper (design-only per R-1) | 🟨 design spec only |
| **P5 Assembly** | Sessions B–G, front/back matter, UBC format | 🟥 0/7 sessions |
| **P6 Reproduce, Defend, Publish** | DOI, Paper 1, defence | ⬜ not started; D undefined |

**Gates G0–G6 (Native Sandbox Methodology):**

| Gate | What | State |
|---|---|---|
| G0 Bring-up 12/12 | Canon O1 | ✅ **reproduced today by the agent** (not operator-executed) |
| G1 SP-1 control rerun | Fresh S1 trace | ✅ **reproduced today** (fresh trace, 13/13 tx gas-identical); operator-executed rerun still owed |
| G2 Compatibility @ 0.8.24 | Compatibility table | 🟨 partial: TEST "unmodified" column filled. **New blocker:** the canon's own compiler isn't really 0.8.24 (see §3, finding 1) |
| G3–G5 T-725/735/740 | SP-2 verdicts | ⬜ not run; `sp2_verdicts.json` absent |
| G6 Evidence merge → Final | Architect | ⬜ |

---

## 3. What this pass discovered (ranked)

1. **The canon's gas numbers are not what they say they are.** The canon sandbox pins `"solc": "^0.8.24"` with no lockfile, so a fresh install resolves **solc 0.8.37**. The subtask override still *labels* that build "0.8.24" in build-info. The committed trace's `createPresentation` = 62,441 gas matches the **0.8.37** build; a true 0.8.24 build gives **62,458 (+17)**. All other 12 transactions are identical. Consequence: every "0.8.24" gas figure in the corpus is actually a newer-compiler figure, and G2's premise ("compatibility at 0.8.24") is ambiguous. *Fix:* pin exactly `"solc": "0.8.24"` plus commit a lockfile, or hash-check our vendored soljson, then re-baseline F8. (`XV_canon_02d`, `05y`)
2. **Hub WO-0 is effectively solved.** `cvin-sandbox-v1.3` (Downloads zip; extracted at `toolchain/parallel-work/`) matches the hub's WO-0 fingerprint, and its trace holds exactly the acceptance numbers (178,935 / 130,163 / 62,441). It is **v1.3, not v1.1**, and has **no `cvin-v6/`**, because cvin-v6 lives in `CVIN-ID/TEST`, which the hub never registered.
3. **The Infura key's true origin is `CVIN-ID/TEST`** (public since 2023-11-07; mirrored in `nikhilprakash-cvin/prevWork_testingIDSystems`). **Our 2026-09-24 pushes copied that history into the public SSI-DID spoke.** The hub's rules call this a P0 defect ("no release tag may precede rotation"; tag `asfound/pre-onboarding` now sits on the spoke). The added exposure is small, since the key was already public, but the hub rules are breached. Remediation needs you (§5).
4. **The hub's model of the world is 1 day stale.** The spoke's `claude/cv2x-testbed-setup-*` branch received ~20 pushes on 09-24, including a July "Release v0.8.0" (`a64c6f3`). The hub also calls itself v0.8.0 → **two different 0.8.0s**. The hub's "spoke is frozen, older than v0.7.0, nothing to merge" is now false for that branch.
5. **Supervisor-facing status overstates prose.** The 08-18 supervisor package says chapters are "Drafted" and Ch3 "core narrative drafted". Same-day internal records say prose is 0/7 and Ch3/Ch4 NOT STARTED. The Sept-3 Memo A v6 also says LFDT "validates", against the locked "corroboration, not validation" wording.
6. **CI is broken in both hub and spoke.** `npm ci` has no lockfile, so it fails. The spoke's matrix glob matches no test files, and the hub CI label says "147 tests" against the real 201. Every "green" claim rests on local `npm install` of floating ranges.
7. **Lineage B gas is not comparable with the Lineage A canon:** hub compiles with `viaIR: true`; canon doesn't. The WO-R map already rules Lineage B numbers "indicative" only; this is the mechanical reason.
8. **Spoke ≠ IMinimalSSI.** The spoke's ERC-1056 contracts natively cover F11/F12 and F3, adapt F1/F2/F9/F10, carry F4 only by convention, and **lack F5–F8**. The 47/47 and the 12/12 are different test regimes that don't map onto each other. This settles the stream B↔C question: an adapter is needed for comparison.
9. **Build-III restore is proven safe (preview):** H3 confirmed (only 3 missing local files), and the upstream `3b1b4935` restore gives 259/259 at Node 16, offline. This de-risks D2 completely.
10. **The TEST repo isn't in the corpus at all** (not in SISTER_REPOS, not in any document), even though the corpus's own key-rotation gate points at a file inside it.

---

## 4. Four streams, one map

```
CVIN-ID/TEST (2023, public, KEY) ─fork→ nikhilprakash24/TEST (onboarding, PRs #1-4 open) ─ also pushed → SPOKE (⚠ collision)
                └ mirror → nikhilprakash-cvin/prevWork_testingIDSystems (KEY)
cvin-sandbox-v1.3 (Downloads, never pushed) = LINEAGE A canon (O1 12/12)  ──should fill──→ HUB sandbox/ slot (WO-0)
HUB cvin-sc bundle v0.8.0 (local; target nikhilprakash-cvin2 = 404) ← imported be32c6e ← nikhilprakash-cvin/2_miniature-waffle v0.7.0
SPOKE nikhilprakash24/CVIN-SC-Implementation-SSI-DID: default (47/47) + cv2x branch (v0.7→"v0.8.0"→today, ahead of hub)
Local VID-Build (vid-build, unpushed) = spoke clone + 5_minimal-ssi-sandbox (canon copy with solc PINNED + VID extensions, untracked)
```

---

## 5. What only you can unblock (ordered by leverage)

| # | Decision / action | Why it matters |
|---|---|---|
| U1 | **Set D-2 / Q5: a submission window**, even provisional | Every plan in the corpus is parameterised on it; the most-blocking item for months |
| U2 | **Rotate the Infura key** (dashboard; review usage since 2023-11) | P0 gate for any release tag. Public ~3 years |
| U3 | **Spoke collision:** approve deleting `sandbox-onboarding`, `wo-s0/*`, `docs/root-readme` and tag `asfound/pre-onboarding` from the SSI-DID spoke, and reverting/keeping `TEST_ONBOARDING.md` | Restores the hub's no-key-history rule. Destructive, so it needs your explicit OK |
| U4 | **Hub remote:** create `nikhilprakash-cvin2`, or amend D-R3 to an existing account | Handoff step 1 is blocked |
| U5 | **Declare v1.3 the WO-0 recovery** (import as `sandbox/cvin-sandbox-v1.3/`) + **choose the compiler truth** (pin 0.8.24 and re-baseline F8, or restate the canon as 0.8.37) | Unblocks WO-R, G2 and the Ch4 plan |
| U6 | **Canonical Lineage B tip:** hub `be32c6e` vs spoke `5a62137` (with July v0.8.0); resolve the duplicate 0.8.0 | Avoids two sources of truth |
| U7 | D2 (Build-III restore) + D4 (V7 toolbox) for the TEST stream | 259/259 preview says it's safe |
| U8 | Supervisor-status correction before the next supervisor contact | Credibility risk (§3, finding 5) |
| U9 | Install Python 3.11/3.12 (system change), or name a machine that has it | The only way to verify 28 VC / 21 MOBI VID / W3C % / SUMO claims |

---

## 6. Parallel work sources read in this pass

- Claude sessions: "CA - CVIN-SC thesis monorepo handoff" (09-24, idle, awaiting D-R3 choice), "CVIN identity system thesis documentation" (09-04), "VID system documentation and scenarios" (08-22), "Sumo traffic visualization plan" (06-21).
- Local: `CVIN-2026-repoPrep/cvin-sc-{0,1}` (hub), `CVIN-2026-VID-Build` (spoke clone + minimal sandbox), `CVIN-2026-Deliverables` (Memo A v6 ×2, Overview A v12, Manual v2.0), `CVIN-2026-Sept-Outputs…`, `CVIN-2026-Thesis` (RW audits), `CVIN-2026-Sandbox`, `CVIN-2026-Resources` (CVIN 2020 paper), Downloads (≈40 docs June–Sept incl. the 08-18 supervisor package, Scaffolding/MWP/MEP, the Writing-1 Suite, the handoff).
- GitHub: `CVIN-ID/{TEST,CVIN-ID-SCs}`, `nikhilprakash24/{TEST,CVIN-SC-Implementation-SSI-DID,CVIN_2024}`, `nikhilprakash-cvin/{2_miniature-waffle…,prevWork_testingIDSystems}`; `nikhilprakash-cvin2` (404).

*Companion documents: `CROSS_VERIFICATION_TEST_REPORT_v1.0.md` (every test, command, number) and `NEXT_STEPS_PLAN_v2.0.md` (programme-level plan).*
