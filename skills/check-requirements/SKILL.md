---
name: check-requirements
description: 'Check-requirements template for verifying prerequisites. Use when the user asks to check installed tools, validate dev environment prerequisites, or create a pre-flight script for project setup.'
---

# Check Requirements

## Template

```bash
#!/usr/bin/env bash
set -euo pipefail

RED='\033[0;31m'
GREEN='\033[0;32m'
NC='\033[0m'

check() {
    if command -v "$1" &>/dev/null; then
        version=$("$1" --version 2>/dev/null | head -1 || echo "inconnue")
        echo -e "  ${GREEN}✓${NC} $1 (${version})"
    else
        echo -e "  ${RED}✗${NC} $1 — introuvable"
        return 1
    fi
}

echo "Vérification des prérequis"
echo ""

ok=0
check node || ok=1
check python3 || ok=1
check docker || ok=1
check git || ok=1

echo ""
if [ "$ok" -eq 0 ]; then
    echo -e "${GREEN}Tous les prérequis sont satisfaits.${NC}"
else
    echo -e "${RED}Certains prérequis sont manquants.${NC}"
    exit 1
fi
```

