---
name: bash-scripting
description: Helps users write, debug, and optimize bash scripts using the Keno Kivabe template
---

# Bash Scripting Skill

Based on the template from [Build a Reusable Bash Script Template](https://blogs.kenokivabe.com/article/build-a-reusable-bash-script-template) by Keno Kivabe.

## When to Use

Use this skill when the user asks you to:
- Write or modify a bash script
- Debug a bash script that isn't working
- Add argument parsing, error handling, or a help menu to a script
- Generate a new script from a reusable template

## The Template

```bash
#!/usr/bin/env bash

# =============================================================================
# SCRIPT NAME: script_template.sh
# DESCRIPTION: Brief explanation of what this script does.
#
# USAGE: ./script_template.sh [-h] [-v] [-f <filename>]
#
# OPTIONS:
#   -h            Display this help message and exit
#   -v            Enable verbose mode
#   -f <file>     Specify an input file
#
# EXAMPLES:
#   ./script_template.sh -v -f data.txt
#
# AUTHOR: Your Name
# VERSION: 1.0.0
# ============================================================================

set -euo pipefail

# ---------------------------
# Global Variables
# ---------------------------
VERBOSE=0
INPUT_FILE=""

# ---------------------------
# Functions
# ---------------------------

usage() {
  awk '/^# =/{p++;next} p==1{print}' "$0" | sed 's/^# //; s/^#$//'
  exit 0
}

error_exit() {
  echo "Error: $1" >&2
  exit 1
}

log() {
  if [[ $VERBOSE -eq 1 ]]; then
    echo "[INFO] $*"
  fi
}

# ---------------------------
# Parse Arguments
# ---------------------------

while getopts ":hvf:" opt; do
  case ${opt} in
    h )
      usage
      ;;
    v )
      VERBOSE=1
      ;;
    f )
      INPUT_FILE="$OPTARG"
      ;;
    \? )
      error_exit "Invalid option: -$OPTARG. Use -h for help."
      ;;
    : )
      error_exit "Option -$OPTARG requires an argument."
      ;;
  esac
done
shift $((OPTIND -1))

# ---------------------------
# Main Script
# ---------------------------

main() {
  if [[ -z "$INPUT_FILE" ]]; then
    error_exit "Input file not specified. Use -f <file>."
  fi

  log "Processing file: $INPUT_FILE"
  # Add your main logic here
}

main "$@"
```

## Key Features

### Auto-Generated Help
The `usage()` function parses the `#` comment block at the top of the script using `awk '/^# =/{p++;next} p==1{print}' "$0" | sed 's/^# //; s/^#$//'`. Update the header comments and the help menu updates automatically — no duplication.

### getopts Argument Parsing
Uses POSIX `getopts` for flag parsing. Format: `while getopts ":hvf:" opt`. The `:` prefix silences errors (handled manually), and `f:` means `-f` takes a required argument.

| getopts syntax | Meaning |
|---|---|
| `h` | flag, no argument |
| `v` | flag, no argument |
| `f:` | flag with required argument |
| `\?` | unknown option |
| `:` | missing argument |

### Error Handling
`error_exit()` prints to stderr and exits 1. Combined with `set -euo pipefail`, the script halts on any unexpected failure.

### Logging
`log()` only prints when `VERBOSE=1`, keeping normal output clean and debug optional.

### main() Flow
All logic lives in `main "$@"` at the bottom — avoids issues with half-parsed state and keeps the script readable.

## Customizing

1. **Save** as `script_template.sh`
2. **Copy** for each new script: `cp script_template.sh my_script.sh && chmod +x my_script.sh`
3. **Edit** the header comment block (help auto-updates)
4. **Add options** by extending the `getopts` case and adding variables
5. **Add functions** in the Functions section
6. **Insert logic** inside `main()`

## Advanced Add-ons (Beyond the Template)

- **ShellCheck**: Run `shellcheck script.sh` to catch issues.
- **Temporary files**: `trap 'rm -rf "$TMPDIR"' EXIT` with `mktemp -d`.
- **Dependency checks**: `command -v "$cmd" &>/dev/null || error_exit "Missing $cmd"`.
- **Arrays**: `files=("$@")` / `for f in "${files[@]}"`.
- **Long options**: Add a manual `while [[ $# -gt 0 ]]; do case "$1" in --file=*) ...` paser alongside `getopts`.
- **Debug mode**: Run with `bash -x script.sh` or wrap sections in `set -x`/`set +x`.

## Common Pitfalls

- Unquoted variables (word splitting, globbing)
- `set -e` + `grep` without `|| true` (grep returns non-zero on no match)
- Using `exit` in a subshell instead of the main script
- Missing `|| true` on commands expected to fail
- Forgetting `chmod +x` after copying the template
