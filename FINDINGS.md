# FINDINGS — CVIN-ID/TEST Sandbox Onboarding

**Status:** Provisional · **Pass:** WO-S0 (inventory & safety sweep) · **Branch:** `sandbox-onboarding` · **Date:** 2026-06-20
**Scope rule:** This file logs *deviations and hazards only* (Standing Rule 4). Empirical compile/test results land here from WO-S1/WO-S2. No secret value is printed (Standing Rule 7) — locations only.

---

## A. Hazards (WO-S0 — surfaced, not acted on)

| # | Hazard | Location (only) | Severity | Action owner | Token |
|---|---|---|---|---|---|
| H1 | **Leaked credential** — Infura RPC key embedded in source | `CVIN-Implementation-II-ERC1056/CVIN-Implementation-1056Testbed-V6/cvin-v6/index.js:19` | High (rotate regardless of liveness) | **Operator** | [TS-4] |
| H2 | **Deprecated network** — code targets Goerli (retired) | `…/cvin-v6/index.js:9,19,20,28` (`chainNameOrId='goerli'`, infura goerli, publicnode goerli) | Med (resolution will fail) | Architect (retarget later) | — |
| H3 | **Broken imports** — Build-III core contracts import `./custom/`, `./interfaces/`, `contracts/helpers/` which are **absent** | `…/Build-III-…/ERC725-develop/implementations/contracts/*.sol` | High (blocks compile) | CLI agent (WO-S1) | [TS-1] |
| H4 | **Duplicate source** — flat copy of Build-III contracts | `…/Build-III-…/SmartContracts/*.sol` (byte-identical to `…/implementations/contracts/`) | Low (double-count risk) | Evaluate one only | — |
| H5 | **Empty shell, not a build** — V7 `contracts/` holds only the Hardhat `Lock.sol` scaffold | `…/cvin-v7/contracts/Lock.sol` | Info (intended SP-1 host, empty today) | Confirm; do not fill this pass | [TS-6] |
| H6 | **Plugin/version mismatch** — V7 config + tests require `@nomicfoundation/hardhat-toolbox` (absent from `package.json`); `deploy.js` uses ethers v6 syntax while ethers is pinned v5 | `…/cvin-v7/hardhat.config.js`, `…/test/Lock.js`, `…/scripts/deploy.js`, `…/package.json` | Med (blocks `hardhat` cmds as-is) | CLI agent (WO-S1) | — |
| H7 | **Non-functional dApp** — `react-dapp` imports a nonexistent `Greeter` artifact, `ethers` not declared, CRA `index.js` entrypoint overwritten | `…/cvin-v7/react-dapp/src/index.js`, `index_template_base.js`, `package.json` | Low (out of scope this pass) | Park | — |
| H8 | **Unbuilt branch** — LSP0 track has no build system; `LSP0ERC725Account/` empty; `LSP2Utils.sol` imports uninstalled `@erc725/smart-contracts` | `CVIN-Implementation-III-LSP0/` | Info | Park | [TS-7] |

> H1 — **Operator only** acts on the key (rotate/remove + move to untracked `.env`). The CLI agent does not echo, commit, or transact with it.

---

## B. Deviations observed at inventory (expectation vs. reality)

- **"CVIN-modified" is a misnomer.** `Build-0/SmartContracts/CVIN_ERC725.sol` is byte-for-byte the standard ERC-725 v1 identity interface (only a trailing newline differs vs the Build-II copy). Contract is still declared `contract ERC725`, pragma `0.4.24`, standard key-purpose constants. **No VIN/vehicle field or logic exists.** The `CVIN_` is a filename prefix only.
- **No subfolder is at the pinned toolchain.** Pragmas span `0.4.24` (Build-0, Build-II), `^0.8.0/^0.8.5/^0.8.17` (Build-III), `^0.8.9 / 0.8.19` (V7). Evaluation target is **solc 0.8.24, offline-pinned**. ⇒ central task is *compatibility triage*, not bring-up.
- **Build-III is reference-only as checked in** (H3) — repo-content gap, not a toolchain issue.
- **V7 is a stock `npx hardhat init` scaffold** — `Lock.sol`, `test/Lock.js`, `scripts/deploy.js`, README are unmodified samples; no ERC-1056 / DID-registry Solidity present.
- **V6 is the verbatim ethr-did quick-start** — DID *creation* runs offline; *resolve()/lookupOwner()* depend on live (dead) Goerli.

---

## C. Placeholder token register (TS-#)

| Token | Class | Question | Authority | Status |
|---|---|---|---|---|
| [TS-1] | Empirical | Build-III ERC725 passes X/Y suite at Node 16.19.0? | CLI agent (WO-S1) + operator | **Open** — blocked by H3 until missing sources restored |
| [TS-2] | Empirical | ERC-725 X/Y compiles at 0.8.24 offline unmodified or only after mechanical port? | CLI agent (WO-S2) | **Open** |
| [TS-3] | Empirical | `CVIN_ERC725.sol` (0.4.24) ports cleanly to 0.8.24; what changes? | CLI agent (WO-S2) | **Open** |
| [TS-4] | Documentary→action | Is in-repo Infura key live? (Rotate regardless.) | **Operator** | **Open** — H1 |
| [TS-5] | Design | Canonical SP-2 ERC-725/735 anchor in `cvin-sandbox-v1.x`? | **Architect** (WO-S4) | **Open** |
| [TS-6] | Design | Minimal scope to make V7 the SP-1 on-chain ERC-1056 host? | Architect + CLI (later WO-1) | **Open** |
| [TS-7] | Scope | Is LSP0 in the evaluation window or parked? | Architect / supervisor | **Open** |

*No documentary source may resolve [TS-1], [TS-2], [TS-3] — these graduate only from an actual offline compile/test log.*

---

## D. Compatibility table @ solc 0.8.24 (offline) — **WO-S2, pending**

| Candidate set | Native pragma | Compiles unmodified | Compiles after mechanical port | Blocked (reason) | Evidence |
|---|---|---|---|---|---|
| Build-III ERC725 X/Y | ^0.8.x | _pending_ | _pending_ | (H3 must be resolved first) | — |
| Build-0 `CVIN_ERC725.sol` | 0.4.24 | _pending_ | _pending_ (expect 0.4→0.8 port) | — | — |
| Build-II Origin contracts | 0.4.24 | _pending_ | _pending_ | — | — |

*(Port notes to capture per row: constructor syntax, `constant`→`view`, explicit visibility, storage-array returns, SPDX header, integer/overflow semantics.)*

---

## E. Evidence index (immutable, timestamped)

- `evidence/WO-S0_filetree_<ts>.txt` — full file tree (excl. `.git`).
- `evidence/WO-S0_pragmas_<ts>.txt` — every `pragma solidity` with path:line.
- `evidence/WO-S0_network_secrets_<ts>.txt` — RPC/network references in source (`.js`/`.ts`), **secret values redacted**.

*Append-only. A new run is a new file (Standing Rule 6).*
