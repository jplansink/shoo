# Shoo 👻

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Fast, offline-first npm supply chain attack scanner. Single bash script, zero dependencies.

Checks your lockfiles, source files, and system for compromised packages, malware, and suspicious patterns.

## Why Shoo?

npm supply chain attacks are increasing. Shoo [detects](#what-it-detects), malware files, exfiltration endpoints, and suspicious install scripts.

| Attack | Packages | Impact |
|--------|----------|--------|
| Axios/UNC1069 | axios@1.14.1, axios@0.30.4 | 100M+ weekly downloads, North Korea |
| SANDWORM_MODE | 19 typosquatting packages | AI toolchain poisoning, worm |
| GlassWorm | 400+ repos, invisible unicode | Blockchain C2, credential theft |
| TeamPCP/Trivy | trivy-action, 66+ npm packages | CI/CD credential theft |
| Cline/Clinejection | cline@2.3.0 | AI coding assistant hijack |
| tj-actions | CVE-2025-30066 | 23K+ repos, CI secret leak |
| nx/Singularity | nx@20.9.0-21.8.0 | Millions of downloads |
| debug/chalk | debug@4.4.2, chalk@5.6.1 | Billions of downloads |
| DuckDB | duckdb@1.3.3, @duckdb/node-api@1.3.3 | Data exfiltration |
| Shai-Hulud | Malware files, workflow tampering | Targeted orgs |

- **Fast** — runs in milliseconds, single bash script, zero dependencies
- **Works offline** — no network required, fetches updates when available
- **Context-aware** — ignores test files and build artifacts, reduces false positives
- **Beyond npm audit** — checks VS Code extensions, shell configs, GitHub workflows

## Installation

```bash
# Download
curl -sO https://raw.githubusercontent.com/jplansink/shoo/main/shoo
chmod +x shoo

# Or clone
git clone https://github.com/jplansink/shoo.git
```

## Requirements

- Bash 4.0+
- Optional: `curl` or `wget` for online malware database

## Example

```
$ ./shoo

Shoo 👻 .

Total: 2 (critical: 1, high: 1)

CRITICAL
  ✗ Compromised version: nx@21.5.0 (nx/Singularity) [CWE-506]
    package-lock.json
    https://cwe.mitre.org/data/definitions/506.html
    → Remove package, clean reinstall, rotate credentials

HIGH
  ✗ Suspicious postinstall script (curl/wget/eval) [CWE-94]
    node_modules/sketchy-pkg/package.json
    https://cwe.mitre.org/data/definitions/94.html
    → Review script contents before running npm install
```

## Usage

```bash
./shoo                # Scan current directory
./shoo ~/projects     # Scan specific directory
./shoo --all          # Scan directory + system
./shoo --system       # Scan system only
./shoo -q             # Quiet (exit code only)
./shoo --json         # JSON output
./shoo --sarif        # SARIF output (GitHub Security)
./shoo --offline      # Skip online databases
```

Exit codes: `0` clean, `1` critical, `2` high, `3` medium

## What It Scans

| Location | What It Checks |
|----------|----------------|
| Lockfiles | Compromised package versions, phantom dependencies |
| Source files | Malware patterns, exfiltration endpoints, GlassWorm markers |
| package.json | Suspicious install scripts, global installs |
| GitHub workflows | Malicious actions, SHA pinning, encoded payloads |
| VS Code extensions | Unicode malware |
| npm cache | Cached malware files |
| Shell configs | Tampering (.bashrc, .zshrc) |
| AI tool configs | MCP server injection (Claude, Cursor, Continue, Windsurf) |
| Git hooks | Persistence scripts in global hook templates |

> [!TIP]
> Use `--system` to scan global packages and VS Code extensions without a project.

## What It Detects

### Known Attacks

| Attack | Packages |
|--------|----------|
| Axios/UNC1069 (Mar 2026) | axios@1.14.1, axios@0.30.4, plain-crypto-js |
| SANDWORM_MODE (Feb 2026) | 19 typosquatting packages (claud-code, suport-color, etc.) |
| GlassWorm (Jan-Mar 2026) | @aifabrix/miso-client, react-native packages, 400+ repos |
| Cline/Clinejection (Feb 2026) | cline@2.3.0 |
| TeamPCP/Trivy (Mar 2026) | trivy-action, 66+ npm packages |
| tj-actions (Mar 2025) | CVE-2025-30066, CVE-2025-30154 |
| Crypto typosquats (Mar 2026) | raydium-bs58, ethersproject-wallet + 3 more |
| eslint-prettier (Jul 2025) | eslint-config-prettier@8.10.1-10.1.7 |
| rand-user-agent (May 2025) | rand-user-agent@1.0.110, 2.0.83, 2.0.84 |
| nx/Singularity | nx@20.9.0-21.8.0 + @nx/* scoped packages |
| debug/chalk | debug@4.4.2, chalk@5.6.1 + 16 more |
| DuckDB | duckdb@1.3.3, @duckdb/node-api@1.3.3 |
| Shai-Hulud 2.0 | posthog-js, @asyncapi/specs, @ensdomains/ensjs |
| XRP | xrpl@4.2.1-4.2.4, @2.14.2 |
| React Native Aria | 16 compromised packages |

Plus 1000+ packages from [aikido.dev](https://www.aikido.dev) online database (cached 24h).

### Suspicious Patterns

| Pattern | Examples |
|---------|----------|
| Malware files | `setup_bun.js`, `bun_environment.js` |
| Phantom dependencies | `plain-crypto-js` (Axios attack payload) |
| Exfiltration endpoints | `webhook.site`, `sfrclak.com`, `pastebin.com` |
| Hardcoded secrets | AWS keys, GitHub/npm tokens, private keys |
| Obfuscated payloads | Base64 blobs, hex encoding, suspicious minification |
| Destructive code | `rm -rf`, recursive rmSync |
| Suspicious postinstall | curl/wget/eval in scripts, `npm install -g` |
| Unicode obfuscation | Hidden homoglyph characters, GlassWorm markers |
| Malicious workflows | Encoded payloads, bad runners |
| Unpinned compromised actions | tj-actions, reviewdog, trivy-action without SHA |
| Shell config tampering | .bashrc, .zshrc modifications |
| MCP server injection | Rogue AI tool configs (SANDWORM_MODE) |
| Git hook persistence | Suspicious global git hook scripts |
| npm audit CVEs | Known vulnerabilities in dependencies |
| Targeted org typosquatting | @asyncapi, @posthog, @zapier, @aifabrix |

### Supported Lockfiles

`package-lock.json` · `yarn.lock` · `pnpm-lock.yaml`

## CI/CD

```yaml
- name: Security scan
  run: |
    curl -sO https://raw.githubusercontent.com/jplansink/shoo/main/shoo
    chmod +x shoo
    ./shoo -q || exit 1
```

### GitHub Security Tab

```yaml
- name: Security scan (SARIF)
  run: |
    curl -sO https://raw.githubusercontent.com/jplansink/shoo/main/shoo
    chmod +x shoo
    ./shoo --sarif > results.sarif || true
- name: Upload SARIF
  uses: github/codeql-action/upload-sarif@v3
  with:
    sarif_file: results.sarif
```

## Excluding Files

Create `.shooignore` in your project:

```
test/fixtures/
malware-samples/
```

## Credits

- [Aikido.dev](https://www.aikido.dev) — Malware database
- [OSV.dev](https://osv.dev) — Google's open vulnerability database
- [Socket.dev](https://socket.dev) — Supply chain research

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on adding new detections.

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for version history.

## License

MIT
