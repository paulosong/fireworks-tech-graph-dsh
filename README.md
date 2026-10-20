# fireworks-tech-graph-dsh

A [DeepSeek Harness](https://deepseek.com) (dsh) plugin that wraps the
[Fireworks Tech Graph](https://github.com/yizhiyanhua-ai/fireworks-tech-graph)
Agent Skill (MIT) so it can be used **from inside any DeepSeek Harness
environment** — installable via the command line, and kept automatically in
sync with upstream.

Fireworks Tech Graph produces geometry-safe technical diagrams: architecture,
UML, sequence, ER, state machine, flowchart, timeline, C4, cloud, event,
observability, agent and workflow diagrams, with **SVG as the canonical
artifact** and PNG / offline HTML / SVG-to-GIF motion as output routes.

> This repository is the *adapter shell*. The actual diagram engine (Python
> CLI, templates, schemas, docs) lives vendored under `skills/fireworks-tech-graph/`
> and is re-synced from upstream automatically. See [NOTICE](./NOTICE) for
> attribution and license details.

---

## Install into a dsh environment

Two ways to obtain the package; the dsh wiring is identical afterwards.

### A. From npm (recommended for stable installs)

```bash
cd ~/.dsh/profiles/<your-profile>     # e.g. desktop
pnpm add fireworks-tech-graph-dsh
```

### B. From GitHub (always tracks the latest, including auto-synced upstream)

```bash
cd ~/.dsh/profiles/<your-profile>
pnpm add github:YOUR_GITHUB_USERNAME/fireworks-tech-graph-dsh
```

> If your dsh CLI exposes `dsh plugin add`, you can equivalently run
> `dsh plugin add fireworks-tech-graph-dsh` (npm) or
> `dsh plugin add github:YOUR_GITHUB_USERNAME/fireworks-tech-graph-dsh`.

### Wire it into the profile (both cases)

1. Ensure `"fireworks-tech-graph-dsh"` is listed in `dsh.profile.bundles`
   of that profile's `package.json`.
2. `pnpm install`.
3. Fully restart the dsh app / reload the profile (Cmd+Q, then reopen).

---

## Host prerequisites

The bundled skill invokes a local Python CLI, so the host machine needs:

- **Python 3.9+** (verified present on macOS/Linux).
- A rasteriser for **PNG** export: `rsvg-convert` (preferred) or `cairosvg`.
- **Optional**, for GIF motion: Node.js 22.12+ with `puppeteer-core`.

---

## Usage

Once loaded, the skill `fireworks-tech-graph` is available to every agent
preset in that profile. Just ask the agent in natural language, e.g.:

> "Draw an architecture diagram for our order-service with a gateway, two
> microservices, and a Postgres primary/replica."

The agent resolves `skills/fireworks-tech-graph/` as `SKILL_ROOT` and runs
`python3 "$SKILL_ROOT/scripts/fireworks.py" ...` to emit SVG/PNG/HTML/GIF.

---

## Repository layout

```
fireworks-tech-graph-dsh/
├─ package.json            # 【adapter】 plugin manifest (name, dsh fields, scripts)
├─ lib/index.js            # 【adapter】 Cordis plugin: registers the skill provider
├─ dsh.plugin.json         # 【adapter】 dsh plugin declaration + contributes.skills
├─ cordis.patch.yml        # 【adapter】 bundle patch
├─ README.md               # 【adapter】 this document
├─ LICENSE                 # upstream MIT license (preserved verbatim)
├─ NOTICE                  # 【adapter】 attribution / license summary
├─ sync_upstream.sh        # 【adapter】 upstream re-vendor script
├─ UPSTREAM_COMMIT         # tracking metadata (rewritten by sync)
├─ .github/workflows/
│  ├─ sync-upstream.yml    # weekly auto-PR on upstream change
│  └─ publish-npm.yml      # publish to npm on GitHub Release
└─ skills/
   └─ fireworks-tech-graph/  # 【upstream, vendored】 the actual skill (SKILL.md, scripts/, …)
```

---

## License

MIT. The upstream skill is MIT (Copyright 2025 fireworks-tech-graph
contributors) and is preserved under `LICENSE` and
`skills/fireworks-tech-graph/LICENSE`. The adapter code is MIT as well
(Copyright 2026 fireworks-tech-graph-dsh authors). See [NOTICE](./NOTICE).
