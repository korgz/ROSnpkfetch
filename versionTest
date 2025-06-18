#!/bin/bash

# Enable strict mode to fail fast on error.
set -euo pipefail

# Constants
DESKTOP_FILE="/home/dell/Desktop/version.txt"
DOWNLOAD_DIR="/home/dell/Seafile/versionTest"
NEWEST_FILE="NEWESTa7.development"
NAME_SUBSTRING="_ab"
BASE_URL="https://hidden/routeros"
LOG_FILE="/home/dell/versionlog"

# Redirect all output to log file
exec > >(tee -a "$LOG_FILE") 2>&1

echo "----------------------------"
echo "$(date '+%Y-%m-%d %H:%M:%S') - Script started"

# Create version file placeholder
touch "$DESKTOP_FILE"
echo "Created $DESKTOP_FILE"

# Find and delete files with matching substring
found_files=$(find "$DOWNLOAD_DIR" -type f -name "*$NAME_SUBSTRING*" || true)
if [ -z "$found_files" ]; then
    echo "No files containing '$NAME_SUBSTRING' found."
else
    while IFS= read -r file; do
        rm -f "$file"
        echo "Deleted: $file"
    done <<< "$found_files"
fi

# Download version info file
echo "Downloading $NEWEST_FILE..."
if ! wget -q "$BASE_URL/$NEWEST_FILE"; then
    echo "ERROR: Failed to download $NEWEST_FILE"
    exit 1
fi

# Extract version string
version=$(awk 'NR==1 { print $1 }' "$NEWEST_FILE")
if [ -z "$version" ]; then
    echo "ERROR: Version string not found in $NEWEST_FILE"
    exit 1
fi

echo "Version detected: $version"

# Platform targets
targets=(
    "arm" "arm64" "mipsbe" "mmips" "ppc" "smips" "tile" "" "x86"
)

# Download routeros packages
for target in "${targets[@]}"; do
    if [ "$target" == "" ]; then
        pkg="routeros-${version}.npk"
    elif [ "$target" == "x86" ]; then
        pkg="routeros-x86-${version}.npk"
    else
        pkg="routeros-${version}-${target}.npk"
    fi
    echo "Downloading: $pkg"
    wget -q -P "$DOWNLOAD_DIR" "$BASE_URL/${version}/${pkg}" && echo "✓ Downloaded: $pkg"
done

# Wireless packages
wireless_targets=(
    "wireless-${version}-arm.npk"
    "wireless-${version}-arm64.npk"
    "wireless-${version}-mipsbe.npk"
    "wireless-${version}-mmips.npk"
    "wireless-${version}-ppc.npk"
    "wireless-${version}-smips.npk"
    "wireless-${version}-tile.npk"
    "wireless-${version}.npk"
    "wifi-qcom-${version}-arm.npk"
    "wifi-qcom-${version}-arm64.npk"
    "wifi-qcom-ac-${version}-arm.npk"
)

for pkg in "${wireless_targets[@]}"; do
    echo "Downloading: $pkg"
    wget -q -P "$DOWNLOAD_DIR" "$BASE_URL/${version}/${pkg}" && echo "✓ Downloaded: $pkg"
done

# Cleanup
rm -f "$NEWEST_FILE"
echo "Removed temporary file: $NEWEST_FILE"

echo "$(date '+%Y-%m-%d %H:%M:%S') - Script completed"
echo "----------------------------"