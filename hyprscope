#!/usr/bin/env bash

set -euo pipefail

CONFIG_FILE="$HOME/.config/hypr/hyprscope.conf"
COMMAND=('gamescope')
declare -A OPTS=([hdr]=0 [hdr_itm]=0 [dry_run]=0)
ENV=()
FLAGS=()
ARGV=()

# Load global flags from file
if [ -f "$CONFIG_FILE" ]; then
  set -- $(
    while IFS= read -r flags; do
      if [ -z "$flags" ] || [[ "$flags" =~ ^[[:space:]]*# ]]; then
        continue
      fi

      echo $flags
    done < "$CONFIG_FILE"
  ) "$@"
fi

# Parse arguments
while [ $# -gt 0 ]; do
  case "$1" in
    -G)
      COMMAND=('gamemoderun' 'gamescope')
      shift
      ;;
    --env)
      ENV+=("$2")
      shift 2
      ;;
    --hdr)
      OPTS[hdr]=1
      shift
      ;;
    --hdr-itm)
      OPTS[hdr_itm]=1
      shift
      ;;
    --dry-run)
      OPTS[dry_run]=1
      shift
      ;;
    --)
      ARGV+=("$1")
      shift
      while [ $# -gt 0 ]; do
        ARGV+=("$1")
        shift
      done
      break
      ;;
    *)
      FLAGS+=("$1")
      shift
      ;;
  esac
done

# Load monitor info
MONITOR_INFO="$(hyprctl monitors -j | jq -r '.[] | select(.focused==true)')"
if [ -z "$MONITOR_INFO" ]; then
  MONITOR_INFO="$(hyprctl monitors -j | jq -r '.[0]')"
fi

# Generate flags
FLAGS+=('-f')
FLAGS+=('-W' "$(echo "$MONITOR_INFO" | jq -r '.width')")
FLAGS+=('-H' "$(echo "$MONITOR_INFO" | jq -r '.height')")
FLAGS+=('-r' "$(echo "$MONITOR_INFO" | jq -r '.refreshRate' | awk '{print int($1)}')")
if [ "$(echo "$MONITOR_INFO" | jq -r '.colorManagementPreset')" = "hdr" ]; then
  if [ "${OPTS[hdr]}" = 1 ]; then
    FLAGS+=('--hdr-enabled')
  fi

  if [ "${OPTS[hdr_itm]}" = 1 ]; then
    FLAGS+=('--hdr-itm-enabled')
  fi
fi

# Prepend env to command
if [ "${#ENV[@]}" -gt 0 ]; then
  COMMAND=('env' "${ENV[@]}" "${COMMAND[@]}")
fi

if [ "${OPTS[dry_run]}" = 1 ]; then
  echo "${COMMAND[@]}" "${FLAGS[@]}" "${ARGV[@]}"
  exit 0
fi

exec "${COMMAND[@]}" "${FLAGS[@]}" "${ARGV[@]}"
