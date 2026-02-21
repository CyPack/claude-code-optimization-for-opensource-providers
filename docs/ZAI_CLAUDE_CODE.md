# z.ai + Claude Code Guide

Last verified: 2026-02-21

This guide documents the repository-supported z.ai path for Claude Code via z.ai's Anthropic-compatible endpoint.

## Official Baseline

From z.ai Claude Code docs:

- `ANTHROPIC_AUTH_TOKEN=<your_zai_api_key>`
- `ANTHROPIC_BASE_URL=https://api.z.ai/api/anthropic`
- Default mapping:
  - `ANTHROPIC_DEFAULT_OPUS_MODEL=GLM-4.7`
  - `ANTHROPIC_DEFAULT_SONNET_MODEL=GLM-4.7`
  - `ANTHROPIC_DEFAULT_HAIKU_MODEL=GLM-4.5-Air`

## Repository Switch Path

```bash
cc-provider zai
cc-provider status
```

Supported alias:

```bash
cc-zai
```

## GLM-5 (Plan-Dependent)

According to z.ai FAQ, GLM-5 availability depends on plan tier.  
If your plan supports it, switch mapping to GLM-5:

```bash
ZAI_SONNET_MODEL=GLM-5 ZAI_OPUS_MODEL=GLM-5 cc-provider zai
```

Fallback to stable defaults:

```bash
cc-provider zai
```

## Pre-Flight Checklist

```bash
cc-provider zai
cc-provider status
token="$(jq -r '.env.ANTHROPIC_AUTH_TOKEN // empty' $HOME/.claude/settings.local.json)"
base="$(jq -r '.env.ANTHROPIC_BASE_URL // empty' $HOME/.claude/settings.local.json)"
ANTHROPIC_AUTH_TOKEN="$token" ANTHROPIC_BASE_URL="$base" \
  claude -p "Reply with exactly: zai_ok" --model sonnet
```

If your command returns `Not logged in`, use the inline env export pattern above for print-mode smoke tests.

## References

- z.ai Claude Code docs:
  - https://docs.z.ai/scenario-example/develop-tools/claude
- z.ai Coding FAQ:
  - https://docs.z.ai/devpack/faq
- z.ai API key page:
  - https://z.ai/manage-apikey/apikey-list
