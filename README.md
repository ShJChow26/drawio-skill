# drawio-skill — From Text to Professional Diagrams

[中文文档](README_CN.md) | [Online Docs](https://agents365-ai.github.io/drawio-skill/)

## What it does

- Generates `.drawio` XML files from natural language descriptions
- Exports diagrams to PNG, SVG, PDF, or JPG using the native draw.io desktop CLI
- **6 diagram type presets**: ERD, UML Class, Sequence, Architecture, ML/Deep Learning, Flowchart — with preset shapes, styles, and layout conventions
- **Animated connectors** (`flowAnimation=1`) for data-flow and pipeline diagrams (visible in SVG and draw.io desktop)
- **ML model diagram support** with tensor shape annotations `(B, C, H, W)` — ideal for NeurIPS/ICML/ICLR papers
- **Grid-aligned layout** — all coordinates snap to 10px multiples for clean alignment
- **Browser fallback** — generates diagrams.net URLs when the desktop CLI is unavailable
- Iterative design: preview, get feedback, and refine diagrams until they look right
- **Auto-launch** draw.io desktop after export for manual fine-tuning
- Triggers automatically when diagrams would help explain complex systems
- **Style presets (v1.3 new)** — teach the skill your visual style from a `.drawio` file or image, save it by name, and apply it to future diagrams. See `## Style Presets` in SKILL.md.

## Multi-Platform Support

Works with all major AI coding agents that support the [Agent Skills](https://agentskills.io) format:

| Platform | Status | Details |
|----------|--------|---------|
| **Claude Code** | ✅ Full support | Native SKILL.md format |
| **Opencode** | ✅ Full support | Native SKILL.md via `skill` tool; also reads `.claude/skills/` paths |
| **OpenClaw / ClawHub** | ✅ Full support | `metadata.openclaw` namespace, dependency gating, ClawHub installer |
| **Hermes Agent** | ✅ Full support | `metadata.hermes` namespace, tags, tool gating |
| **OpenAI Codex** | ✅ Full support | `agents/openai.yaml` sidecar file |
| **SkillsMP** | ✅ Indexed | GitHub topics configured |

## Comparison

### vs No Skill (native agent)

| Feature | Native agent | This skill |
|---------|-------------|------------|
| Generate draw.io XML | Yes — LLMs know the format | Yes |
| Self-check after export | No | Yes — reads PNG and auto-fixes 6 issue types |
| Iterative review loop | No — must manually re-prompt | Yes — targeted edits, 5-round safety valve |
| Proactive triggers | No — only when explicitly asked | Yes — auto-suggests when 3+ components |
| Layout guidelines | None — varies by run | Complexity-scaled spacing, routing corridors, hub placement |
| Grid alignment | No | Yes — all coordinates snap to 10px multiples |
| Diagram type presets | No | Yes — 6 presets (ERD, UML, Sequence, Architecture, ML/DL, Flowchart) |
| Animated connectors | No | Yes — `flowAnimation=1` for data-flow visualization |
| ML model diagrams | No | Yes — tensor shape annotations, layer-type color coding |
| Color palette | Random/inconsistent | 7-color semantic system (blue=services, green=DB, purple=auth...) |
| Edge routing rules | Basic | Pin entry/exit points, distribute connections, waypoint corridors |
| Container/group patterns | None | Swimlane, group, custom container with parent-child nesting |
| Embed diagram in export | No | Yes — `--embed-diagram` keeps exported PNG/SVG/PDF editable |
| Browser fallback | No | Yes — generates diagrams.net URL when CLI unavailable |
| Auto-launch desktop app | No | Yes — opens `.drawio` file after export for fine-tuning |

### vs Other draw.io Skills & Tools

| Feature | This skill | [jgraph/drawio-mcp](https://github.com/jgraph/drawio-mcp) (official, 1.3k⭐) | [bahayonghang/drawio-skills](https://github.com/bahayonghang/drawio-skills) (60⭐) | [GBSOSS/ai-drawio](https://github.com/GBSOSS/ai-drawio) (63⭐) |
|---------|-----------|---------------|-------------------|--------------|
| **Approach** | Pure SKILL.md | SKILL.md / MCP / Project | YAML DSL + MCP | Plugin + browser |
| **Dependencies** | draw.io desktop only | draw.io desktop | MCP server (`npx`) | Browser + local server |
| **Multi-agent** | ✅ 6 platforms | ❌ Claude Code only | ❌ Claude Code only | ❌ |
| **Self-check** | ✅ 2-round auto-fix | ❌ | ❌ | ❌ screenshot |
| **Iterative review** | ✅ 5-round loop | ❌ generate once | ✅ 3 workflows | ❌ |
| **Layout guidance** | ✅ complexity-scaled + grid snap | ✅ basic spacing | ❌ relies on MCP | ❌ |
| **Diagram presets** | ✅ 6 types (ERD, UML, Seq, Arch, ML, Flow) | ❌ | ❌ | ❌ |
| **Animated edges** | ✅ `flowAnimation=1` | ❌ | ❌ | ❌ |
| **ML/DL diagrams** | ✅ tensor shapes, layer colors | ❌ | ❌ | ❌ |
| **Color system** | ✅ 7-color semantic | ❌ | ✅ 5 themes | ❌ |
| **Container/group** | ✅ swimlane + group | ✅ detailed | ❌ | ❌ |
| **Embed diagram** | ✅ `--embed-diagram` | ✅ | ❌ | ❌ |
| **Edge routing** | ✅ corridors + waypoints | ✅ arrowhead rules | ❌ | ❌ |
| **Browser fallback** | ✅ diagrams.net URL | ❌ | ❌ | ❌ |
| **Auto-launch** | ✅ opens desktop app | ❌ | ❌ | ❌ |
| **Cloud icons** | AWS basic | ❌ | ✅ AWS/GCP/Azure/K8s | ❌ |
| **Zero-config** | ✅ copy SKILL.md | ✅ | ❌ needs `npx` | ❌ needs plugin install |

### Key advantages

1. **Self-check + iterative loop** — the only pure-SKILL.md solution that reads its own output and auto-fixes before showing the user, then supports multi-round refinement
2. **6 diagram type presets** — ERD, UML Class, Sequence, Architecture, ML/Deep Learning, Flowchart — each with preset shapes, styles, and layout conventions
3. **ML/DL model diagrams** — tensor shape annotations, layer-type color coding, encoder/decoder swimlanes — built for academic papers
4. **Multi-agent, zero-config** — works across 6 platforms with just one `SKILL.md` file + draw.io desktop. No MCP server, no Python, no Node.js, no browser
5. **Production-grade layout** — grid-aligned coordinates, complexity-scaled spacing, routing corridors, hub-center strategy, animated connectors
6. **Browser fallback** — generates diagrams.net URLs when the desktop CLI is unavailable, plus auto-launch for desktop editing

## Supported diagram types

- **Architecture**: microservices, cloud (AWS/GCP/Azure), network topology, deployment — with tier-based swimlanes and hub-center strategy
- **ML / Deep Learning**: Transformer, CNN, LSTM, GRU architectures — with tensor shape annotations and layer-type color coding
- **Flowcharts**: business processes, workflows, decision trees, state machines — with semantic shape types (parallelogram I/O, diamond decisions)
- **UML**: class diagrams (inheritance/composition/aggregation arrows), sequence diagrams (lifelines, activation boxes)
- **Data**: ER diagrams (table containers, PK/FK notation), data flow diagrams (DFD)
- **Other**: org charts, mind maps, wireframes

## How it works

<p align="center">
  <img src="assets/workflow.png" width="420" alt="Workflow">
</p>

## Prerequisites

The draw.io desktop app must be installed for diagram export:

### macOS

```bash
# Recommended — Homebrew
brew install --cask drawio

# Verify
drawio --version
```

### Windows

Download and install from: https://github.com/jgraph/drawio-desktop/releases

```powershell
# Verify
"C:\Program Files\draw.io\draw.io.exe" --version
```

### Linux

Download `.deb` or `.rpm` from: https://github.com/jgraph/drawio-desktop/releases

```bash
# Headless export (required on Linux servers without display)
sudo apt install xvfb  # Debian/Ubuntu
xvfb-run -a drawio --version
```

| Platform | Extra step |
|----------|------------|
| **macOS** | No extra steps after Homebrew install |
| **Windows** | Use full path if not in PATH |
| **Linux** | Wrap commands with `xvfb-run -a` for headless export |

## Skill Installation

### Claude Code

```bash
# Global install (available in all projects)
git clone https://github.com/Agents365-ai/drawio-skill.git ~/.claude/skills/drawio-skill

# Project-level install
git clone https://github.com/Agents365-ai/drawio-skill.git .claude/skills/drawio-skill
```

### Opencode

```bash
# Global install (Opencode-native path)
git clone https://github.com/Agents365-ai/drawio-skill.git ~/.config/opencode/skills/drawio-skill

# Project-level install
git clone https://github.com/Agents365-ai/drawio-skill.git .opencode/skills/drawio-skill
```

Opencode also reads `~/.claude/skills/` and `.claude/skills/`, so an existing Claude Code install is automatically picked up — no second clone needed.

### OpenClaw

```bash
# Via ClawHub
clawhub install drawio-pro-skill

# Manual install
git clone https://github.com/Agents365-ai/drawio-skill.git ~/.openclaw/skills/drawio-skill

# Project-level install
git clone https://github.com/Agents365-ai/drawio-skill.git skills/drawio-skill
```

### Hermes Agent

```bash
# Install under design category
git clone https://github.com/Agents365-ai/drawio-skill.git ~/.hermes/skills/design/drawio-skill
```

Or add an external directory in `~/.hermes/config.yaml`:

```yaml
skills:
  external_dirs:
    - ~/myskills/drawio-skill
```

### OpenAI Codex

```bash
# User-level install
git clone https://github.com/Agents365-ai/drawio-skill.git ~/.agents/skills/drawio-skill

# Project-level install
git clone https://github.com/Agents365-ai/drawio-skill.git .agents/skills/drawio-skill
```

### SkillsMP

Browse on [SkillsMP](https://skillsmp.com/skills/agents365-ai-drawio-skill-skill-md) or use the CLI:

```bash
skills install drawio-skill
```

### ClawHub

Browse on [ClawHub](https://clawhub.ai/agents365-ai/drawio-pro-skill) or use the CLI:

```bash
clawhub install drawio-pro-skill
```

### Installation paths summary

| Platform | Global path | Project path |
|----------|-------------|--------------|
| Claude Code | `~/.claude/skills/drawio-skill/` | `.claude/skills/drawio-skill/` |
| Opencode | `~/.config/opencode/skills/drawio-skill/` (also reads `~/.claude/skills/`) | `.opencode/skills/drawio-skill/` (also reads `.claude/skills/`) |
| OpenClaw / ClawHub | `~/.openclaw/skills/drawio-skill/` | `skills/drawio-skill/` |
| Hermes Agent | `~/.hermes/skills/design/drawio-skill/` | Via `external_dirs` config |
| OpenAI Codex | `~/.agents/skills/drawio-skill/` | `.agents/skills/drawio-skill/` |
| SkillsMP | N/A (installed via CLI) | N/A |

## Updates

The skill auto-checks for updates once per 12 hours on first use in a conversation. When a new version is available, the agent prints a one-line notice in the reply. To apply:

```bash
cd <your-install-path>/drawio-skill && git pull
```

The check is read-only, self-throttled, and silent when up to date, offline, or not a git install — it won't block or slow the workflow.

Package-manager installs handle updates themselves:

```bash
# OpenClaw
clawhub update drawio-pro-skill

# SkillsMP
skills update drawio-skill
```

## Usage

Just describe what you want:

```
Create a microservices e-commerce architecture with API Gateway, auth/user/order/product/payment services,
Kafka message queue, notification service, and separate databases for each service
```

The agent will generate the `.drawio` XML file and export it to PNG automatically.

## Example

**Prompt:**
> Create a microservices e-commerce architecture with Mobile/Web/Admin clients, API Gateway,
> Auth/User/Order/Product/Payment services, Kafka message queue, Notification service,
> and User DB / Order DB / Product DB / Redis Cache / Stripe API

**Output:**

![Microservices Architecture](assets/microservices-example.png)

## Topology demos

The skill handles various diagram topologies with clean edge routing — no lines crossing through shapes.

### Star topology (7 nodes)

Central message broker with 6 microservices radiating outward. Edges enter Kafka from different sides, zero crossings.

![Star topology](assets/demo-star.png)

### Layered flow (10 nodes, 4 tiers)

E-commerce architecture with 2 cross-connections: Order→Product (same-tier horizontal) and Auth→Redis (diagonal via routing corridor). All edges route cleanly.

![Layered flow](assets/demo-layered.png)

### Ring / cycle (8 nodes)

CI/CD pipeline with a closed loop and 2 spur branches. Edges flow along the perimeter without crossing the interior.

![Ring cycle](assets/demo-ring.png)

## Files

- `SKILL.md` — **the only required file**. Loaded by all platforms as the skill instructions.
- `agents/openai.yaml` — OpenAI Codex-specific configuration (UI, policy)
- Auto-update is inline in `SKILL.md` (step 0): a 24h-throttled `git pull --ff-only` on first use per conversation
- `README.md` — this file (English, displayed on GitHub homepage)
- `README_CN.md` — Chinese documentation
- `assets/` — example diagrams and workflow images

> **Note:** Only `SKILL.md` is needed for the skill to work. `agents/openai.yaml` is only needed for Codex. The `assets/` and README files are documentation only and can be safely deleted to save space.

> All example diagrams were generated by Claude Opus 4.6 with this skill.

## Known Limitations

- **Command name varies by platform**: macOS Homebrew installs the command as `draw.io`; some Linux packages use `drawio`. The skill handles both, but verify which name works on your system with `draw.io --version` or `drawio --version`
- **Linux headless export**: Requires `xvfb` for display virtualization — without it, CLI export will fail on servers without a display
- **Browser fallback requires Python**: The `diagrams.net` URL generator uses `python3` for compression/encoding. If neither CLI nor Python is available, the skill generates `.drawio` XML only
- **Self-check requires vision**: The auto-fix step reads exported PNGs using the model's vision capability. Models without vision support skip this step
- **Cloud icons**: Currently supports basic AWS resource icons. GCP, Azure, and Kubernetes icon sets are not yet included
- **No source .drawio for microservices example**: The `microservices-example.png` in assets was generated in a session but the source `.drawio` was not preserved

## License

MIT

## Support

If this skill helps you, consider supporting the author:

<table>
  <tr>
    <td align="center">
      <img src="https://raw.githubusercontent.com/Agents365-ai/images_payment/main/qrcode/wechat-pay.png" width="180" alt="WeChat Pay">
      <br>
      <b>WeChat Pay</b>
    </td>
    <td align="center">
      <img src="https://raw.githubusercontent.com/Agents365-ai/images_payment/main/qrcode/alipay.png" width="180" alt="Alipay">
      <br>
      <b>Alipay</b>
    </td>
    <td align="center">
      <img src="https://raw.githubusercontent.com/Agents365-ai/images_payment/main/qrcode/buymeacoffee.png" width="180" alt="Buy Me a Coffee">
      <br>
      <b>Buy Me a Coffee</b>
    </td>
  </tr>
</table>

## Author

**Agents365-ai**

- Bilibili: https://space.bilibili.com/441831884
- GitHub: https://github.com/Agents365-ai
