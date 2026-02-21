# Kimi Swarm Optimization

Date: 2026-02-21

## Objective

Raise `kimi-for-coding` multi-agent reliability for spawn, orchestration, and result collection.

## Applied Optimizations

1. `ToolSearch` hard-disabled in runtime path.
2. `ENABLE_TOOL_SEARCH` set to `"0"` in `$HOME/.claude/settings.json`.
3. Added robust command: `/custom:kimi-swarm`.
4. Enforced `agentId`-based `TaskOutput` collection (no synthetic IDs).
5. Wave-based spawn pattern (`4+4+2` for 10 agents) to reduce sibling-call fragility.
6. Added global autonomous orchestration rule:
   - `$HOME/.claude/rules/autonomous-orchestration.md`
   - Complex tasks now prefer automatic multi-agent spawn in normal chat (no explicit slash command required).

## Why This Helps

- Prevents Kimi `ToolSearch -> tool_reference` error path from contaminating orchestration.
- Reduces false-success summaries where agents are done but outputs are not collected.
- Makes partial-result scenarios explicit and recoverable.

## Files Changed

- `$HOME/.claude/settings.json`
- `$HOME/.claude/commands/custom/kimi-swarm.md`
- `$HOME/.claude/rules/autonomous-orchestration.md`

## Backup

- `$HOME/.claude/backups/settings.json.20260221-115839.pre-swarm-opt.bak`

## Recommended Usage

```bash
claude -p "/custom:kimi-swarm Large codebase quality scan --agents 10" --dangerously-skip-permissions
```

Or in interactive Claude Code:

```text
/custom:kimi-swarm Large codebase quality scan --agents 10
```

## Health Check

```bash
jq '.env.ENABLE_TOOL_SEARCH' $HOME/.claude/settings.json
jq '.permissions.deny | index("ToolSearch")' $HOME/.claude/settings.json
claude -p "One line: model id only" --output-format text --dangerously-skip-permissions
```
