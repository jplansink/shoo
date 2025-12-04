# Changelog

All notable changes to Shoo will be documented in this file.

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
