# Higgsfield UGC Playbook

An end-to-end Claude Code skill for producing UGC and marketing video ads using **Claude + Higgsfield MCP**.

Covers the full pipeline from creative strategy through generation and iteration — including the MCSLA prompt framework, 6 hook archetypes, Soul ID character setup, QA checklist, and failure mode fixes.

---

## Prerequisites

### 1. Higgsfield MCP

This skill drives Claude to call Higgsfield's API via MCP. Install the MCP server first:

```bash
claude mcp add higgsfield -- npx -y @higgsfield-ai/higgsfield-mcp
```

Or add manually to your MCP config:

```json
{
  "mcpServers": {
    "higgsfield": {
      "command": "npx",
      "args": ["-y", "@higgsfield-ai/higgsfield-mcp"],
      "env": {
        "HIGGSFIELD_API_KEY": "your_api_key_here"
      }
    }
  }
}
```

Get your API key and full MCP setup docs at [higgsfield.ai/mcp](https://higgsfield.ai/mcp).

### 2. Higgsfield account with credits

| Action | Credit cost |
|--------|-------------|
| 4× Seedance 2.0 variants (12s) | ~30–60 credits |
| 4× Veo 3.1 variants (12s) | ~80–150 credits |
| Soul ID training (one-time/character) | ~40 credits |
| Nano Banana 2 image | ~3–8 credits |

---

## Install the Skill

```bash
mkdir -p ~/.claude/skills/higgsfield-ugc-playbook
curl -o ~/.claude/skills/higgsfield-ugc-playbook/SKILL.md \
  https://raw.githubusercontent.com/hivijc/higgsfield-ugc-playbook/main/SKILL.md
```

Then add to `~/.claude/CLAUDE.md`:

```markdown
# higgsfield-ugc-playbook
- **higgsfield-ugc-playbook** (`~/.claude/skills/higgsfield-ugc-playbook/SKILL.md`) - UGC video ad playbook for Claude + Higgsfield MCP. Trigger: `/higgsfield-ugc-playbook`
When working on Higgsfield video generation or UGC ads, consult this skill automatically.
```

---

## What's Inside

### The 6-Phase Pipeline

| Phase | Output | Frequency |
|-------|--------|-----------|
| 1. Strategy Brief | `BRIEF.md` Claude can reason from | Once per product |
| 2. Asset Library | Soul ID + product refs + brand kit | Once per product |
| 3. Ideation | 6 hook variations × archetypes | Per ad batch |
| 4. Prompt Construction | MCSLA-structured Higgsfield prompt | Per ad |
| 5. Generation & QA | 4 variants + 11-point checklist | Per ad |
| 6. Iteration | AI feedback loop + hook variation testing | Ongoing |

### MCSLA Prompt Framework

Every Higgsfield prompt structured in 5 layers:

```
M — Model:   Which Higgsfield model (Seedance 2.0, Veo 3.1, Kling 3.0, etc.)
C — Camera:  Angle, motion, lens (selfie handheld, dolly-in, FPV, etc.)
S — Subject: Soul ID character + outfit + expression + props
L — Look:    Aesthetic, color grade, lighting, film stock, aspect ratio
A — Action:  Beat-by-beat with timestamps (Shot 1 (0–3s):, Shot 2 (3–6s):, etc.)
```

### 6 Hook Archetypes (0–3 seconds)

| # | Archetype | Pattern |
|---|-----------|---------|
| 1 | Bold claim | "[Surprising fact that flips a default assumption]" |
| 2 | Called-out problem | "If you've ever [frustration], this is for you" |
| 3 | Curiosity gap | "I tried this for [time] and..." |
| 4 | Question | "Why do [audience] always [behavior]?" |
| 5 | Disqualifier | "Don't [try this] if [filter]" |
| 6 | Demo-first visual | Cold open showing product working before any words |

### Model Selection

| Model | Best For |
|-------|----------|
| **Seedance 2.0** | Default for UGC — multi-shot, identity consistency |
| **Kling 3.0** | Best lip-sync for dialogue-heavy ads |
| **Veo 3.1** | Hero campaigns — top realism + native audio |
| **Nano Banana 2** | UI close-ups with readable text |
| **Marketing Studio** | URL-driven product ads |

### QA Checklist

```
[ ] Hook lands in first 3 seconds
[ ] Brand name pronounced correctly
[ ] CTA spoken and visible
[ ] Soul ID consistent across shots
[ ] Product UI readable, no glitches
[ ] No uncanny artifacts
[ ] Mute test passes (85% of feed video watched without sound)
```

### Failure Mode Fixes

| Symptom | Fix |
|---------|-----|
| Brand name mispronounced | Hyphenate: `"pan-tree"`, `"mister-white"` |
| Face shifts between shots | Verify `soul_id` is passed; reduce style preset |
| Plastic/CGI skin | Add `"no 3D, natural skin texture, 35mm film grain"` |
| Lip sync off | Switch to Kling 3.0 or Veo 3.1 |
| UI text is gibberish | Use Nano Banana 2 for UI close-ups |
| Flat energy | Explicit emotion arc: `"Shot 1: deadpan → Shot 3: relief"` |

---

## Quick Start

Once the MCP is connected and the skill is installed:

```
Using my [product] brief and Soul ID "[name]", generate a 12s 9:16
Seedance 2.0 UGC ad with a "called-out problem" hook about [pain point],
ending with the CTA "[phonetic CTA]". 4 variants.
```

For the full pipeline — brief template, Soul ID training, worked examples for two different products, hook variation testing, and the AI feedback loop — see [`SKILL.md`](./SKILL.md).

---

## Sources

Built from:
- [Higgsfield Seedance 2.0 prompting guide](https://higgsfield.ai/blog/seedance-prompting-guide)
- [Higgsfield Soul ID docs](https://higgsfield.ai/blog/SOUL-ID-Superior-Level-of-AI-Character-Consistency)
- [Higgsfield MCP setup](https://higgsfield.ai/mcp)
- [OSideMedia higgsfield-ai-prompt-skill](https://github.com/OSideMedia/higgsfield-ai-prompt-skill) — MCSLA framework
- [AKCodez higgsfield-claude-skills](https://github.com/AKCodez/higgsfield-claude-skills) — UGC pipeline
- [QalaLabs claude-higgsfield-mcp](https://github.com/QalaLabs/claude-higgsfield-mcp) — MCP server
