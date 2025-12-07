# Changelog

All notable changes to Shoo will be documented in this file.

## [2.3.0] - 2025-12-07

### Added
- **Historic attack detection** - 10+ additional compromised package versions from 2018-2022:
  - event-stream@3.3.6 (Bitcoin wallet stealer, 2018)
  - flatmap-stream@0.1.1 (event-stream attack payload)
  - ua-parser-js@0.7.29/0.8.0/1.0.0 (crypto miner + password stealer, Oct 2021)
  - coa@2.0.3/2.0.4 (data exfiltration, Nov 2021)
  - rc@1.2.9 (data exfiltration, Nov 2021)
  - colors@1.4.44-liberty-2 (sabotage, Jan 2022)
  - faker@6.6.6 (sabotage, Jan 2022)

- **Extended exfiltration domains**:
  - pipedream.net, requestbin.com, burpcollaborator.net, ngrok.io, localtunnel.me

- **Enhanced shell config attack detection**:
  - curl/wget piped to shell (remote script execution)
  - base64 decoded execution (obfuscated commands)
  - eval with variables (code injection)
  - Suspicious PATH modifications (backdoor persistence)

- **Targeted flatmap-stream lockfile scanning** - Defense-in-depth for 2018 event-stream attack

### Changed
- Shell config scanning now checks .bash_profile and .profile in addition to .bashrc/.zshrc
- Improved eval detection with exclusions for direnv, conda, nvm, rbenv, pyenv, homebrew

### Security
- Addresses gaps identified in security audit vs Trivy/Gitleaks
- Covers major historic npm supply chain attacks from 2018-2022
- Detects advanced shell configuration persistence techniques

## [2.2.0] - 2025-12-04

### Added
- **OSV.dev integration** - Queries Google's malware database (MAL-* entries) for real-time detection
- **SARIF output** - `--sarif` flag for GitHub Security tab integration
- **Obfuscation detection** - Detects Base64 payloads, hex-encoded strings, suspicious minification

### Improved
- Better offline/online messaging in help text

## [2.1.0] - 2025-12-04

### Added
- **npm audit integration** - Automatically runs `npm audit` and includes CVE findings
- **Secret detection** - AWS keys, GitHub tokens, npm tokens, private keys
- **JSON output** - `--json` flag for CI integration
- **--all flag** - Scan directory AND system in one command

### Improved
- Reduced false positives for sandbox/template/example directories
- Targeted org warnings demoted to LOW severity
- Context-aware secret detection (test files marked LOW)

### New Detections
- XRP compromise (xrpl@4.2.1-4.2.4, @2.14.2)
- React Native Aria compromise (16 packages)
- Shai-Hulud 2.0 (Nov 2025) - posthog-js, @postman/tunnel-agent, @asyncapi/specs, etc.
- nx@20.12.0 (missing from Singularity)
- 16 additional debug/chalk packages

### CWE Additions
- CWE-798: Use of Hard-coded Credentials
- CWE-1035: Vulnerable Third Party Component

## [2.0.0] - 2025-11-26

### Added
- Three-phase architecture: SCAN, ANALYZE, REPORT
- Context-aware file classification (test files, build artifacts, caches)
- Confidence scoring for findings
- Online malware database integration (aikido.dev)
- 24-hour caching for malware database
- Support for package-lock.json, yarn.lock, and pnpm-lock.yaml
- GitHub workflow scanning
- VS Code extension scanning
- Shell config scanning (.bashrc, .zshrc, .profile)
- Verbose mode for medium/low severity findings
- Offline mode (--offline flag)
- System-only scan (--system flag)

### Detection Coverage
- Shai-Hulud (worm) attack
- nx/Singularity attack
- debug/chalk compromise
- DuckDB compromise
- Unicode/homoglyph malware
- Targeted org attacks (@asyncapi, @posthog, @postman, @zapier, etc.)
- Exfiltration domain detection
- Malicious postinstall scripts

### Known Compromised Versions
- nx@21.5.0, 20.9.0, 20.10.0, 21.6.0, 20.11.0, 21.7.0, 21.8.0
- debug@4.4.2
- chalk@5.6.1
- ansi-styles@6.2.2
- duckdb@1.3.3, @duckdb/node-api@1.3.3, @duckdb/duckdb-wasm@1.29.2

## [1.0.0] - 2025-11-20

### Added
- Initial release
- Basic npm supply chain attack detection
- Single bash script with zero dependencies
