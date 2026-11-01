# Sing-box 1.15 alpha migration gate

Target: upstream SagerNet/sing-box v1.15.0-alpha.13 (2026-10-24).
Baseline: github.com/sagernet/sing-box v1.14.2 replaced by github.com/shtorm-7/sing-box-extended v1.14.1-extended-2.7.2.

## Blocking incompatibilities
- The current source uses a third-party extended fork of sing-box, plus patched sing, sing-vmess, sing-mux, wireguard-go, tailscale, DNSCrypt and related modules. Removing only the sing-box replacement does NOT guarantee compatibility.
- Upstream sing-box 1.15 changes the TUN implementation and deprecates the inbound `stack` field. Inspect config generation and remove `stack` only after compatibility tests. See https://sing-box.sagernet.org/migration/#migrate-tun-stack
- The existing CI builds the application using more than just `go build`: Windows Qt, core, assets and packaging need to be validated separately.
- Windows TUN needs admin rights, real adapter, sleep/wake and DNS regression tests. GitHub-hosted runners cannot establish all of these conditions.
- Keep the main branch and all production builds on the current known baseline until the alpha passes.

## Required work order
1. Map all imported packages from the extended fork; identify non-upstream features that must be ported or gated.
2. Resolve dependency versions with `go mod tidy` in a disposable workspace, then commit both `go.mod` and `go.sum` together.
3. Compile the core on Windows x64, run `go test ./...` and a CLI smoke test.
4. Update GUI config generation for the new TUN behavior; regression-test config parsing and route rules.
5. Build Qt Windows x64 client with pinned toolchain, verify bundled binaries and DLLs.
6. Run real Windows stability tests before labeling any crash fixed.

Do not publish binaries or configure auto-update until steps 1–6 pass.
