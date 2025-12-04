# Shoo 👻

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Fast, offline-first npm supply chain attack scanner. Single bash script, zero dependencies.

Checks your lockfiles, source files, and system for compromised packages, malware, and suspicious patterns.

## Why Shoo?

npm supply chain attacks are increasing. Shoo [detects](#what-it-detects), malware files, exfiltration endpoints, and suspicious install scripts.

| Attack | Packages | Impact |
|--------|----------|--------|
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

HIGH
  ✗ Suspicious postinstall script (curl/wget/eval) [CWE-94]
    node_modules/sketchy-pkg/package.json

Use -v for remediation steps
```

## Usage

```bash
./shoo                # Scan current directory
./shoo ~/projects     # Scan specific directory
./shoo --all          # Scan directory + system
./shoo --system       # Scan system only
./shoo -v             # Verbose output
./shoo -q             # Quiet (exit code only)
./shoo --offline      # Skip online database
```

Exit codes: `0` clean, `1` critical, `2` high, `3` medium

## What It Scans

| Location | What It Checks |
|----------|----------------|
| Lockfiles | Compromised package versions |
| Source files | Malware patterns, exfiltration endpoints |
| package.json | Suspicious install scripts |
| GitHub workflows | Malicious actions, encoded payloads |
| VS Code extensions | Unicode malware |
| npm cache | Cached malware files |
| Shell configs | Tampering (.bashrc, .zshrc) |

> [!TIP]
> Use `--system` to scan global packages and VS Code extensions without a project.

## What It Detects

### Known Attacks

| Attack | Packages |
|--------|----------|
| nx/Singularity | nx@20.9.0-21.8.0 |
| debug/chalk | debug@4.4.2, chalk@5.6.1, ansi-styles@6.2.2 |
| DuckDB | duckdb@1.3.3, @duckdb/node-api@1.3.3 |
| Shai-Hulud | Malware files, workflow tampering |

Plus 1000+ packages from [aikido.dev](https://www.aikido.dev) online database (cached 24h).

### Suspicious Patterns

| Pattern | Examples |
|---------|----------|
| Malware files | `setup_bun.js`, `bun_environment.js` |
| Exfiltration endpoints | `webhook.site`, `pastebin.com` |
| Destructive code | `rm -rf`, recursive rmSync |
| Suspicious postinstall | curl/wget/eval in scripts |
| Unicode obfuscation | Hidden homoglyph characters |
| Malicious workflows | Encoded payloads, bad runners |
| Shell config tampering | .bashrc, .zshrc modifications |
| Credential harvesting | Token/key extraction |
| Targeted org typosquatting | @asyncapi, @posthog, @zapier |

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

## Excluding Files

Create `.shooignore` in your project:

```
test/fixtures/
malware-samples/
```

## Credits

- [Aikido.dev](https://www.aikido.dev) — Malware database
- [Socket.dev](https://socket.dev) — Supply chain research

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on adding new detections.

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for version history.

## License

MIT
