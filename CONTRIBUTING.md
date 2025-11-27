# Contributing to Shoo 👻

## Adding New Detections

### Malware Files

Add known malware filenames to the `MALWARE_FILES` array:

```bash
readonly -a MALWARE_FILES=(
    "setup_bun.js"
    "bun_environment.js"
    "actionsSecrets.json"
    # Add new entries here
)
```

### Compromised Package Versions

Add to `BAD_VERSIONS` with format `package@version|attack_name`:

```bash
readonly BAD_VERSIONS="
nx@21.5.0|nx/Singularity
debug@4.4.2|debug/chalk compromise
# Add new entries here
"
```

### Exfiltration Domains

Add to `EXFIL_DOMAINS` regex pattern:

```bash
readonly EXFIL_DOMAINS="webhook\.site|pastebin\.com|newdomain\.com"
```

### Targeted Organizations

Add to `TARGETED_ORGS` regex pattern:

```bash
readonly TARGETED_ORGS="@asyncapi|@posthog|@neworg"
```

### Pattern Detection

For new patterns, add detection in `scan_source_files()` and handling in `analyze_phase()`:

**1. Emit from scan:**
```bash
if grep -qE "new_pattern" "$file" 2>/dev/null; then
    echo "NEW_PATTERN_TYPE|$file|$file_type"
fi
```

**2. Handle in analyze:**
```bash
NEW_PATTERN_TYPE)
    emit_finding "HIGH" "$file" "Description" "detector_name" "0.9" \
        "Recommended action"
    ;;
```

## Pull Request Guidelines

1. One detection type per PR when possible
2. Include source/reference for the threat
3. Test on a real codebase before submitting
4. Update README.md detection tables if adding new attack coverage

## Testing

```bash
# Quick test (system only)
./shoo --system --offline

# Test on a directory
./shoo --offline /path/to/test

# Verbose output
./shoo -v --offline /path/to/test
```

## Questions

Open an issue for discussion before large changes.
