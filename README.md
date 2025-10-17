#!/usr/bin/env bash

SRC_DIR="$HOME/1"
DST_DIR="$HOME/2"
SRC_FILE="$SRC_DIR/tere.txt"

# Loo sihtkaust, kui seda pole
mkdir -p "$DST_DIR"

echo "Jälgin faili $SRC_FILE muudatusi..."

# Kui tere.txt muutub, kopeeritakse see automaatselt kausta 2
inotifywait -m -e close_write "$SRC_FILE" | while read -r path action file; do
    cp "$SRC_FILE" "$DST_DIR/"
    echo "Fail kopeeritud $(date)"
done
