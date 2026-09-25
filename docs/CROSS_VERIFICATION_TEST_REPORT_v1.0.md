# Cross-Verification Test Report (XV pass)

**v1.0 · 2026-09-24/25 UTC · host: Windows 11 Pro, Git Bash · executors: CLI-agent engineers (not operator-executed gates)**
**Rule set:** real measurements only; originals never modified (all runs in `toolchain/runs/*` scratch copies); npm registry the only network; no testnet/RPC; `--ignore-scripts` on every install (never needed to drop it). Evidence: `evidence/XV_*` (63 files, 1.1 MB, secret-swept clean, SHA-256 in `EVIDENCE_MANIFEST.txt`).

## Scoreboard

| # | Target | Claim under test | Result | Verdict |
|---|---|---|---|---|
| XV-1 | Canon sandbox v1.3 (O1_ERC1056) | 12/12 compliance, S1 gas | **12 passing**; S1 12/12 functions; **13/13 tx gas-identical, total 1,168,146 Δ0**; schema 0 violations | ✅ REPRODUCES |
| XV-1b | Canon compiler truth | "solc 0.8.24" | `^0.8.24` resolves to **0.8.37**; build-info mislabels it as 0.8.24; true 0.8.24 → F8 **+17 gas** | ❌ **FINDING F-XV-1** |
| XV-2 | Hub `cvin-sc` bundle `2de795c` | integrity | SHA256SUMS **241/241**; tree = bundle HEAD | ✅ |
| XV-3 | Hub Lineage B `1_blockchain-identity` | 201/201, gas 63/63, security 43/43 | **201 passing**; gas table cell-identical; 54 outcomes = 43 DEFENDED + 11 N/A, identical | ✅ REPRODUCES |
| XV-4 | Hub `cv2x-testbed` | (none stated) | compiles (0.8.20); **0 tests exist** | ⚠ green-but-empty |
| XV-5 | Spoke SSI-DID default @`084edfd` | 47/47 + gas table | **47 passing** (19/19/4/1/4) on Node 18 & 24; all 6 function gas figures exact; deploy gas exact under LF checkout | ✅ REPRODUCES |
| XV-6 | Spoke & hub CI | green CI | `npm ci` fails (no lockfile); spoke matrix glob matches no files; `package.json` test scripts broken; hub label "147" vs 201 | ❌ **FINDING F-XV-2** |
| XV-7 | Python layers (VC 28, MOBI VID 21, use cases 12, W3C 93.2%/89.6%, SUMO 0.27 ms) | — | **not runnable**: no real Python on host | ⚪ UNVERIFIED |
| XV-8 | TEST Build-III as-found (0.8.17, 0.8.24) | H3 | fails only on 3 missing local sources (10 errors); flat copy identical | ✅ H3 PROVEN |
| XV-9 | TEST Build-0/Build-II @ 0.8.24 unmodified | [TS-2]/[TS-3] "unmodified" column | all fail on version pragma (0.4.x); Build-II also needs uninstalled `openzeppelin-solidity` | ✅ column filled (FAIL) |
| XV-10 | TEST Build-III **restore preview** (scratch; upstream `3b1b4935` custom/interfaces/helpers, 22 files) | [TS-1] preview | compiles clean at 0.8.17 **and** 0.8.24+opt200; **Hardhat suite 259 passing, 0 failing**, Node 16, fully offline | ✅ PREVIEW (does **not** resolve [TS-1]; needs D2) |
| XV-11 | Build-0/II native | offline compile | impossible: no 0.4.x compiler vendored (needs solc 0.4.24+commit.e67f0147) | ⚠ FINDING F-XV-3 |
| XV-12 | Spoke vs IMinimalSSI | B↔C mapping | none implement IMinimalSSI; ERC-1056: F3/F11/F12 native, F1/F2/F9/F10 adapted, F4 convention, **F5–F8 absent** | 📐 architecture finding |

## XV-1 Canon sandbox v1.3: detail

- Env: system Node v24.16.0 / npm 11.13.0; `npm install --ignore-scripts` → 643 pkgs; **no lockfile in source** → hardhat 2.29.1, toolbox 5.0.0, **solc 0.8.37**.
- `npx hardhat compile` → "Compiled 12 Solidity files successfully (evm target: paris)". BRINGUP says 9: v1.3 added 7 empty O2–O8 placeholder files. Verbose trace: `solcjs:run`, no download.
- `npx hardhat test` → **12 passing (6s)**.
- `IMPL=O1_ERC1056 npx hardhat run scenarios/run_i2i.js` → new trace `i2i_O1_ERC1056_1790294916643.jsonl`, 22 entries.

| fn | committed | reproduced | Δ |
|---|---|---|---|
| createDID ×3 | 178935 / 161835 / 161823 | same | 0 |
| updateDID | 44760 | 44760 | 0 |
| deactivateDID | 55127 | 55127 | 0 |
| issueCredential ×2 | 130163 ×2 | same | 0 |
| revokeCredential | 32980 | 32980 | 0 |
| createPresentation | 62441 | 62441 | 0 |
| addVerificationMethod ×2 | 59607 ×2 | same | 0 |
| removeVerificationMethod | 36478 | 36478 | 0 |
| addDelegate | 54227 | 54227 | 0 |
| **Total** | **1,168,146** | **1,168,146** | **0** |

Only the F8 txHash differs, because its challenge comes from `crypto.randomBytes(8)`. Undocumented `phase` field present in both traces (schema gap).

**F-XV-1 (compiler):** controlled test with fixed inputs, two runs each: 0.8.37 → F8 62,441; vendored release 0.8.24+commit.e11b9ed9 → **62,458**; other 12 tx identical. The committed canon figure is therefore a ≥0.8.25-class build number presented as 0.8.24. Note that `VID-Build/5_minimal-ssi-sandbox/package.json` already pins `"solc": "0.8.24"`, so someone saw this; the fix never reached the canon. (`XV_canon_01c`, `02d`, `05x`, `05y`)

## XV-3 Hub Lineage B: detail

- Node 18.20.4 (the CI pin; Hardhat warns unsupported, results unaffected). `npm ci` exit 1 (no lockfile), so `npm install --ignore-scripts` → 1130 pkgs (hardhat 2.29.1, ethers 6.17.0).
- Compile: 42 files, solc 0.8.24+commit.e11b9ed9, **viaIR true**, opt 200, paris.
- Test: **201 passing (14s)**. `scripts/benchmark_gas.js`: 9-standard table cell-identical to committed; only `metadata.date` differs.
- Hazard: `git clone cvin-sc.bundle` with host `core.autocrlf=true` rewrites bytes (incl. a .docx), so SHA256SUMS then fail. Use `-c core.autocrlf=false`.

## XV-5 Spoke: detail

- HEAD `084edfd`; `708302a` snapshot commit is an ancestor. Node 18.20.4 main, Node 24 cross-check.
- Suites: EthereumDIDRegistry 19, CVINVehicleDIDRegistry 19, ERC721 combined 4 / identity 1 / extended 4.
- Gas: vehicle DID create 78,068; changeOwner 68,854; addDelegate 72,219; setAttribute 51,126; transferVehicleOwnership 57,146; ERC-721 mint avg 102,804. All exact.
- Deploy EthereumDIDRegistry 958,714 (CRLF checkout) vs 958,726 (LF checkout = claim); the 12-gas difference is one metadata-hash byte flipping zero/non-zero.
- Doc drift: PROJECT_SUMMARY "31 contracts" vs snapshot/observed 32; QUICKSTART "18 passing" vs 19.

## XV-8…11 TEST offline triage: detail

- T1 (0.8.17 exe, offline): `implementations/contracts/*` → 10 × `Source "contracts/custom/OwnableUnset.sol" not found` (8 sites), `interfaces/IERC725X.sol` (1), `interfaces/IERC725Y.sol` (1). Only `constants.sol`, `errors.sol` compile.
- T2 (0.8.24): the same for Build-III; Build-0 and all 7 Build-II files → `Source file requires different compiler version`.
- T3 preview: 13/13 as-found files byte-match upstream `3b1b4935`. 22 restored files, SHA-256 listed (CRLF and LF variants) in `XV_testtriage_T3a2_*`. 35-file compile PASS at 0.8.17 and at 0.8.24+opt200 (one deprecation warning in test helper `selfdestruct.sol`). Hardhat 2.13.1 @ Node 16.19.0 with a scratch-only config override → vendored exe, proxy pointed at a dead port: **"Compiled 42 Solidity files … 259 passing (2m)"**. Deploy gas: ERC725 2,414,518 · ERC725X 1,852,895 · ERC725Y 1,134,143.
- T4: Build-0/II need solc 0.4.24 (available on binaries.soliditylang.org, not vendored) + `openzeppelin-solidity ^1.10`.

## What is NOT verified (honest gaps)

1. Everything Python: VC 28/28, MOBI VID 21/21, use cases 12/12, W3C compliance 93.2% (hub) / 89.6% (spoke), SUMO warm-verify p95 0.27 ms.
2. Operator-executed G0/G1: ours are agent reproductions.
3. [TS-1]: preview only, pending D2.
4. Any 0.4.x compile; any port; any SP-2 behaviour (G3–G5).
5. Reproducibility of any result under a *committed* lockfile: none exists in canon, hub (`1_blockchain-identity`) or spoke. Every pass today used floating resolutions.
