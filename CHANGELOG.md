# Changelog

All notable changes to Shoo will be documented in this file.

## [3.0.0] - 2026-04-01

### Added
- **Axios/UNC1069 compromise detection** (Mar 2026) - North Korea-nexus attack on axios with 100M+ weekly downloads:
  - axios@1.14.1, axios@0.30.4, plain-crypto-js@4.2.1
  - Phantom dependency detection: `plain-crypto-js` should never appear in lockfiles (like flatmap-stream)
  - C2 domain: sfrclak.com

- **SANDWORM_MODE worm detection** (Feb 2026) - 19 typosquatting packages with worm-like self-propagation:
  - claud-code, cloude-code, cloude, crypto-locale, detect-cache, format-defaults, hardhta,
    locale-loader-pro, naniod, node-native-bridge, opencraw, parse-compat, rimarf, scan-store,
    secp256, suport-color, veim, yarsg, crypto-reader-info

- **AI toolchain MCP server injection detection** - Scans Claude Desktop, Cursor, VS Code Continue,
  and Windsurf configs for rogue MCP servers injected by SANDWORM_MODE

- **GlassWorm unicode malware detection** (Jan-Mar 2026) - Invisible unicode payloads across 400+ repos:
  - @aifabrix/miso-client@4.7.2, @iflow-mcp/watercrawl-watercrawl-mcp@1.3.0-1.3.4
  - Marker variable detection (`lzcdrtfxyqiplpd`)
  - Persistence file detection (~/init.json)

- **React Native package hijack detection** (Mar 2026) - GlassWorm-linked account takeovers:
  - react-native-international-phone-number@0.11.8, 0.12.1-0.12.3
  - react-native-country-select@0.3.91

- **Cline/Clinejection compromise detection** (Feb 2026):
  - cline@2.3.0 (silently installs OpenClaw via postinstall)
  - New detection: postinstall scripts that run `npm install -g` (global install)

- **TeamPCP/Trivy campaign indicators** (Mar 2026):
  - C2 domain: scan.aquasecurtiy.org
  - aquasecurity/trivy-action added to compromised actions list

- **GitHub Actions SHA pinning audit** - Flags known-compromised actions used without commit SHA pinning:
  - tj-actions/changed-files (CVE-2025-30066)
  - reviewdog/action-setup (CVE-2025-30154)
  - aquasecurity/trivy-action (CVE-2026-33634)
  - aquasecurity/setup-trivy

- **Git hook persistence detection** - Scans global git hook templates for suspicious scripts
  (SANDWORM_MODE persistence vector)

- **Crypto typosquat stealer detection** (Mar 2026):
  - raydium-bs58, base-x-64, bs58-basic, base_xd, ethersproject-wallet

- **eslint-config-prettier phishing compromise** (Jul 2025):
  - eslint-config-prettier@8.10.1, 9.1.1, 10.1.6, 10.1.7

- **rand-user-agent RAT** (May 2025):
  - rand-user-agent@1.0.110, 2.0.83, 2.0.84

- **Extended nx/Singularity scoped packages**:
  - @nx/devkit, @nx/js, @nx/node, @nx/workspace, @nx/eslint, @nx/enterprise-cloud, @nx/key

- **60+ new compromised package versions** across 11 attack campaigns

### Changed
- Version bump to 3.0.0 (major: new detection categories added)
- Extended exfiltration domains: sfrclak.com, scan.aquasecurtiy.org, 0x9c.xyz, checkmarx.zone
- Extended targeted organizations: @aifabrix, @iflow-mcp, @usebioerhold8733, @shadanai, @qqbrowser
- System scan now checks AI tool configs and git hooks

### Security
- Covers all major npm supply chain attacks through March 2026
- Addresses new attack vectors: AI toolchain poisoning, blockchain C2, phantom dependencies
- Detects North Korea-nexus (UNC1069) and GlassWorm threat actor campaigns

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
