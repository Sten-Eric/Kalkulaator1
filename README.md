#!/usr/bin/env bash
set -euo pipefail

PARENT_DIR="$(cd "$(dirname "$0")/.." && pwd 2>/dev/null || echo "$HOME")"
SRC_DIR="$PARENT_DIR/1"
DST_DIR="$PARENT_DIR/2"
SRC_FILE="$SRC_DIR/tere.txt"

# Veendu, et sihtkaust on olemas
mkdir -p "$DST_DIR"

# Tee algkoopia (valikuline, aga kasulik)
if [ -f "$SRC_FILE" ]; then
  cp -f "$SRC_FILE" "$DST_DIR/"
  echo "Algkoopia tehtud: $SRC_FILE -> $DST_DIR/ ( $(date) )"
fi

echo "Jälgin $SRC_FILE muudatusi (close_write/modify/attrib/moved_to)..."

# Jälgi kausta 1 ja filtreeri sündmused, mis puudutavad just tere.txt
inotifywait -m -e close_write,modify,attrib,move,create --format '%f %e' "$SRC_DIR" | while read -r FNAME EVENTS; do
  if [ "$FNAME" = "tere.txt" ]; then
    # Fail võib olla ajutiselt “poolik” -> proovi kuni õnnestub lugeda/kopeerida
    for i in {1..5}; do
      if cp -f "$SRC_FILE" "$DST_DIR/"; then
        echo "Muudatus \"$EVENTS\": kopeeritud $SRC_FILE -> $DST_DIR/ ( $(date) )"
        break
      else
        sleep 0.2
      fi
    done
  fi
done
