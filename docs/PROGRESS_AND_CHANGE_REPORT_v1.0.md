# CVIN-ID/TEST — Progress & Change Report

**Pass:** WO-S0 (Inventory & Safety Sweep) + Node/toolchain foundation · **Role:** CLI agent → operator handback
**Branch:** `sandbox-onboarding` · **Workspace:** `C:\Users\nikhilp\Desktop\CVIN-2026-Sanbox1_v6` · **Date:** 2026-06-20 · **Status:** Provisional

> Scope honored: read-only safety sweep + environment foundation only. **No `npm install` on any project** (that is WO-S1, operator-supervised). No secret value printed or committed. No system Node altered. Work is on a branch; nothing committed yet (awaiting your go-ahead).

---

## 1. What this pass did (in one paragraph)

Established a clean, isolated sandbox foundation for the `CVIN-ID/TEST` onboarding: consolidated the working clone into the sandbox folder on a `sandbox-onboarding` branch, ran the WO-S0 read-only inventory + hazard sweep and captured it as immutable evidence, seeded `FINDINGS.md` and a Provisional Understanding Report, and stood up a **portable, per-shell Node toolchain** (16.19.0 + 18.20.4) that honours Build-III's pin **without touching your system Node 24**. The foundation is verified working.

---

## 2. Foundation status — verified ✅

| Component | Result | Notes |
|---|---|---|
| **System probe** | node `v24.16.0`, npm `11.13.0`, git `2.54.0`, **winget `1.28`** present | yarn/pnpm/**docker** absent; `asdf`/`nvm` absent; `python` is the Store stub |
| **Connectivity** | `registry.npmjs.org`, `github.com`, `nodejs.org` all reachable (TCP 443) | npm installs + git-URL dep fetch (Build-III) will work when WO-S1 runs |
| **Working clone** | `…\CVIN-2026-Sanbox1_v6\TEST` on branch `sandbox-onboarding` | was `main`, 1 commit `0ab45bb first commit`, clean tree |
| **Portable Node 16.19.0** | ✅ `node v16.19.0 / npm 8.19.3` | plan pin for **Build-III** (`.tool-versions`) |
| **Portable Node 18.20.4** | ✅ `node v18.20.4 / npm 10.7.0` | workhorse for **cvin-v6 / cvin-v7** |
| **Isolation check** | activating portable Node 16 resolves `node` to the sandbox copy; **system `C:\Program Files\nodejs\node.exe` still reports `v24.16.0`** | fully reversible — delete `toolchain\` to undo |

---

## 3. Change log (what changed on disk)

| Action | Path | Reversible? |
|---|---|---|
| Moved recon clone → sandbox | `Desktop\CVIN-TEST` → [CVIN-2026-Sanbox1_v6\TEST](CVIN-2026-Sanbox1_v6/TEST) | yes (move back) |
| Created working branch | `sandbox-onboarding` (off `main`) | yes (`git checkout main`) |
| Captured WO-S0 evidence | [TEST\evidence\](CVIN-2026-Sanbox1_v6/TEST/evidence) (3 files, timestamped) | append-only |
| Seeded findings | [TEST\FINDINGS.md](CVIN-2026-Sanbox1_v6/TEST/FINDINGS.md) | yes |
| Provisional report | [TEST\docs\UNDERSTANDING_TEST_REPO.md](CVIN-2026-Sanbox1_v6/TEST/docs/UNDERSTANDING_TEST_REPO.md) | yes |
| Portable Node toolchain | [CVIN-2026-Sanbox1_v6\toolchain\](CVIN-2026-Sanbox1_v6/toolchain) (node 16 + 18) | yes (delete folder) |
| Activation scripts | [use-node16.ps1](CVIN-2026-Sanbox1_v6/use-node16.ps1), [use-node18.ps1](CVIN-2026-Sanbox1_v6/use-node18.ps1) | yes |

**Not done (deliberately):** no `npm install`, no compile, no test, no git commit, no network calls from project code, no action on the leaked key.

---

## 4. WO-S0 results — inventory & hazards

**The big picture:** `CVIN-ID/TEST` is a **survey of three identity standards**, assembled from canonical upstream reference implementations — *not* a built vehicle-identity system. The "CVIN-modified" contract is byte-identical to standard ERC-725 (a filename prefix only). No subfolder sits at the pinned **solc 0.8.24** target, so the core job is **compatibility triage**, not "just run it." Full per-build detail: [UNDERSTANDING_TEST_REPO.md](CVIN-2026-Sanbox1_v6/TEST/docs/UNDERSTANDING_TEST_REPO.md).

**Hazards surfaced (full table in [FINDINGS.md](CVIN-2026-Sanbox1_v6/TEST/FINDINGS.md) §A):**
- **H1 / [TS-4] — leaked Infura key** at `cvin-v6/index.js:19` (location only; **operator rotates**).
- **H2 — deprecated Goerli** in V6 (resolution will fail).
- **H3 / [TS-1] — Build-III won't compile as checked in** (missing `custom/`, `interfaces/`, `helpers/` dirs).
- **H4 — duplicate** flat copy of Build-III contracts (evaluate one).
- **H5 / [TS-6] — V7 is an empty Hardhat shell** (`Lock.sol` sample only).
- **H6 — V7 plugin/version mismatch** (`hardhat-toolbox` absent; ethers v5/v6 clash).
- **H7 — non-functional react-dapp**; **H8 / [TS-7] — LSP0 unbuilt**.

**Evidence (immutable, timestamped) in [TEST\evidence\](CVIN-2026-Sanbox1_v6/TEST/evidence):**
`WO-S0_filetree_*.txt`, `WO-S0_pragmas_*.txt`, `WO-S0_network_secrets_*.txt` (secret values redacted).

---

## 5. Node/nvm foundation — what was set up & why

The plan pins **Node 16.19.0** for Build-III, but your machine runs **Node 24** system-wide. Rather than install nvm-windows (which would have to take over `C:\Program Files\nodejs`, needs UAC elevation, and requires a fresh shell before it's usable), I laid a **portable per-shell foundation** — safer, instantly usable, fully reversible, and aligned with the plan's isolation/offline ethos.

**How to use it (per terminal):**
```powershell
cd C:\Users\nikhilp\Desktop\CVIN-2026-Sanbox1_v6
. .\use-node16.ps1   # Build-III work  (note the leading dot — dot-source)
# ...or...
. .\use-node18.ps1   # cvin-v6 / cvin-v7 work
```
Each script prepends the chosen Node to **this shell's** PATH only and prints the active versions. Open a new shell (or just don't dot-source) to return to system Node 24.

**Optional — system-wide nvm instead.** If you'd rather manage Node globally, winget is ready:
```powershell
winget install CoreyButler.NVMforWindows   # approve the UAC prompt; then open a NEW terminal
nvm install 16.19.0; nvm install 18.20.4
```
⚠️ nvm-windows will offer to take control of your existing Node 24 at `C:\Program Files\nodejs`. Say yes only if you want nvm to manage all Node versions. Tell me and I'll walk it through.

---

## 6. Deliverables produced

1. [TEST\FINDINGS.md](CVIN-2026-Sanbox1_v6/TEST/FINDINGS.md) — hazards, deviations, **[TS-1…TS-7] token register**, empty WO-S2 compatibility table (to fill).
2. [TEST\docs\UNDERSTANDING_TEST_REPO.md](CVIN-2026-Sanbox1_v6/TEST/docs/UNDERSTANDING_TEST_REPO.md) — Provisional; inventory matrix + per-build characterisation (empirical fields tokenized).
3. [TEST\evidence\](CVIN-2026-Sanbox1_v6/TEST/evidence) — 3 immutable WO-S0 logs.
4. Portable Node toolchain + activation scripts (above).
5. This report.

*Per the plan, the formal `docs/ADR-0001_TEST_consolidation.md` and the populated compatibility table are WO-S2/WO-S4 outputs — not this pass.*

---

## 7. Next steps (gated, in order)

| Step | Work order | What happens | Node | Gate |
|---|---|---|---|---|
| **A** | finish WO-S0 | (optional) `git commit` the WO-S0 artifacts on the branch — *awaiting your OK* | — | pre-G0 |
| **B** | **WO-S1** | Build-III: `npm ci` → `npm run build` → `npm test` at **Node 16.19.0**; capture logs. **Expect H3 compile failure** → record as finding [TS-1], do **not** patch upstream. Then V7: confirm the empty `Lock` scaffold compiles (add `hardhat-toolbox` first). | 16 / 18 | G0 |
| **C** | **WO-S2** | Scratch Hardhat @ **solc 0.8.24 offline**; triage Build-III X/Y, Build-0, Build-II → fill the compatibility table; resolve [TS-2]/[TS-3]. | 16/18 | G2 |
| **D** | **WO-S3** | F1–F12 IMinimalSSI **expectation** matrices for ERC-725 & ERC-1056 anchors (needs ISetA/B/C signatures from you/architect). | — | Stage-2 overlay |
| **E** | **WO-S4** | Consolidation ADR — recommend SP-2 anchor ([TS-5]) + V7 SP-1 scope ([TS-6]) + V6/LSP0 disposition. Architect ratifies. | — | architect merge |

**Decisions I need from you (operator/architect) before/within WO-S1:**
- **[TS-4]** Rotate/remove the Infura key (operator-only action).
- Approve restoring Build-III's missing sources from upstream `ERC725Alliance/ERC725` (needed before [TS-1] can pass) — or confirm we log it as a blocked finding and move on.
- Confirm you want me to **commit** the WO-S0 artifacts on the branch.

---

## 8. Startup guide (resume from a fresh terminal)

```powershell
# 0) Go to the sandbox
cd C:\Users\nikhilp\Desktop\CVIN-2026-Sanbox1_v6

# 1) Confirm the branch + clean state
git -C .\TEST status
git -C .\TEST branch --show-current        # -> sandbox-onboarding

# 2) Activate the right Node for the task (per shell)
. .\use-node16.ps1                          # Build-III  (Node 16.19.0)
#   . .\use-node18.ps1                       # cvin-v6 / cvin-v7 (Node 18.20.4)

# 3) WO-S1 — Build-III native bring-up (EXPECT the H3 missing-import failure; capture it)
cd .\TEST\CVIN-Implementation-I-ERC725-735\Build-III-FullERC725StandardCodeBaseAndImplementation\ERC725-develop\implementations
npm ci                                       # falls back to: npm install (log if ci fails)
npm run build  *> ..\..\..\..\evidence\buildIII_native_compile.log    # hardhat compile
npm test       *> ..\..\..\..\evidence\buildIII_native_test.log

# 4) WO-S1 — V7 empty-shell check (Node 18; add the missing toolbox first)
cd ..\..\..\..\CVIN-Implementation-II-ERC1056\CVIN-Implementation-1056Testbed-V7\cvin-v7
npm install
npm install --save-dev @nomicfoundation/hardhat-toolbox
npx hardhat compile  *> ..\..\..\evidence\v7_scaffold_compile.log
npx hardhat test     *> ..\..\..\evidence\v7_scaffold_test.log
```
**Safety reminders during WO-S1:** local EVM only (Hardhat in-process / `npx hardhat node`); no testnet transactions; do **not** run `cvin-v6/index.js` (live Goerli) or the `react-dapp` this pass; if any step fails, that's a *finding* in `FINDINGS.md`, not a thing to patch.

> Say the word and I'll kick off **WO-S1** (Build-III bring-up on Node 16) and report the evidence back here.
