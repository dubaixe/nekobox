# Aria Neko Client — Development Baseline

Status: planning and audit only. No executable is certified or published.

## Scope
- Windows 11 x64 first; Qt UI retained.
- Review upstream sing-box and Xray-core releases, security notes and ABI/config compatibility before pinning versions.
- Preserve GPL-3.0 and dependency licenses.
- Support protocols only after runtime verification; do not claim protocol support based solely on upstream names.
- No real VPN configurations, credentials, server IPs or secrets in this public repository.

## Initial audit findings
1. Existing .github/workflows/build.yml has workflow_dispatch input publish defaulting to true. Never trigger it until a safe non-publishing CI workflow is created and reviewed.
2. Build workflow explicitly checks out main; development branch builds need a separate branch-aware workflow.
3. Third-party submodules: quickjs-ng/quickjs and libcpr/cpr; audit pinned revisions and licenses.
4. Investigate upstream issues #354 (libcronet packaging), #270 (TUN crash), and open pull requests #353, #352 and #349 before applying fixes.

## Planned phases
1. Record current core versions, hashes, dependencies, license inventory and reproducible baseline build.
2. Introduce non-publishing Windows x64 CI with restricted permissions, pinned action commits and artifact retention.
3. Upgrade cores one at a time with compatibility tests, checksum verification and rollback.
4. Reproduce crashes and add regression tests for TUN toggle, system proxy, sleep/wake, DNS and network transitions.
5. Package a test-only binary; do not publish a GitHub Release without explicit approval.

## Acceptance gates
- 100 connect/disconnect cycles, 20 TUN toggles, 10 sleep/wake cycles, 10 network switches.
- DNS leak and routing tests; no credentials in logs.
- Crash dumps redacted and locally retained.
- Rollback verified before updater enabled.

## Governance
All changes occur on dev/aria-neko-v1. Keep main unchanged until review. No public releases or publishing to package registries.