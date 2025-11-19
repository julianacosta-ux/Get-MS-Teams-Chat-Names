# Get-MS-Teams-Chat-Names
This will allow to get a complete lis of all chats within your org.  Allowing for an easy comparison to get a full list of all chat that are taking place.  


To run this scrip:
bash ScripName.sh



#!/usr/bin/env bash
set -euo pipefail

OUTPUT_FILE="${1:-$HOME/teams_chat_names.txt}"

# Ensure mgc exists
if ! command -v mgc >/dev/null 2>&1; then
echo "Microsoft Graph CLI (mgc) not installed."
echo "Install it with: brew install microsoftgraph/graph/microsoft-graph-cli"
exit 1
fi

# Log in (first time opens browser)
mgc login --scopes Chat.Read

echo "Collecting chat names..."

mgc chats list \
--select "topic" \

--output json | jq -r '.value[].topic // ""' > "$OUTPUT_FILE"
echo "Done. Chat names saved to: $OUTPUT_FILE"
