#!/bin/zsh
# description: Markdown to HTML preview (cached)

if [ -z "$1" ]; then
  echo "Usage: md <file.md>"
  exit 1
fi

INPUT="$1"

if [ ! -f "$INPUT" ]; then
  echo "File not found: $INPUT"
  exit 1
fi

DIR=$(dirname "$INPUT")
BASE=$(basename "$INPUT" .md)

OUT_DIR="$DIR/.md_cache"
mkdir -p "$OUT_DIR"

HTML="$OUT_DIR/$BASE.html"

CSS="$HOME/.config/md/qiita.css"

# HTML生成
if [ ! -f "$HTML" ] || [ "$INPUT" -nt "$HTML" ]; then
  echo "Generating HTML..."
  pandoc "$INPUT" -o "$HTML" \
    --css="$CSS" \
    --standalone \
    --syntax-highlighting=pygments
fi

# command
open "$HTML" &
