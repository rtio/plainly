# plainly

Skills for making technical text easier to read, for Codex, Claude Code, Cursor, Gemini CLI, GitHub Copilot, OpenCode, and Windsurf. Includes Claude Code evals.

## Skills

### dumb-down-this-for-me

Explains a technical source (URL, file, or pasted text) at one of four levels. The levels come from [Andrej Karpathy's post](https://x.com/karpathy/status/2105819303471976479) of 2 October 2026:

1. `writing` - ASD-STE100 (Simplified Technical English), 80% of the way by default
2. `diagram` - an SVG diagram, published to Spacefast
3. `html` - an interactive page, published to Spacefast
4. `video` - a 3Blue1Brown-style explainer with narration

```
/dumb-down-this-for-me https://example.com/post            # level 1, 80% STE
/dumb-down-this-for-me https://example.com/post 1 100      # full ASD-STE100
/dumb-down-this-for-me https://example.com/post diagram
/dumb-down-this-for-me https://example.com/post html
/dumb-down-this-for-me https://example.com/post video
```

Levels 2 and 3 need the [Spacefast](https://spacefast.com) CLI (`sf`) signed in, and a team: name one or set `SPACEFAST_TEAM`. Pages are private, with a view-only share link.

Level 4 uses ElevenLabs when `ELEVENLABS_API_KEY` is set and the source is public. Otherwise it uses a local voice (Kokoro, or the Siri voices on macOS), Playwright for frames, and ffmpeg or AVFoundation to encode.

### tighten-this-pr-for-me

Rewrites a pull request description so it says what changed and why, keeps the repo's PR template, and runs through no-ai-slop. It also works on PR comments and commit bodies.

```
/tighten-this-pr-for-me 123
/tighten-this-pr-for-me <pasted description>
```

It never edits the PR on GitHub unless you ask.

### no-ai-slop

[Peter Yang's no-ai-slop](https://github.com/petergyang/no-ai-slop), included unmodified so `tighten-this-pr-for-me` works out of the box. The source commit is in `skills/no-ai-slop/UPSTREAM.md`.

## Install

### Codex and other agents

The skills use the [Agent Skills format](https://agentskills.io/specification). With Node.js 22.20 or later, run the [skills CLI](https://github.com/vercel-labs/skills) from your project and choose your agents:

```sh
npx skills@latest add rtio/plainly
```

Install all three skills for Codex across your projects:

```sh
npx skills@latest add rtio/plainly --skill '*' --agent codex --global
```

Or install them for several agents in the current project:

```sh
npx skills@latest add rtio/plainly --skill '*' \
  --agent codex claude-code cursor gemini-cli github-copilot opencode windsurf
```

Add `--global` for a user-wide install or `--yes` to skip installer prompts. To check what is available before installing:

```sh
npx skills@latest add rtio/plainly --list
```

If selecting individual skills, install `no-ai-slop` with `tighten-this-pr-for-me`:

```sh
npx skills@latest add rtio/plainly --agent codex \
  --skill tighten-this-pr-for-me no-ai-slop
```

Ask your agent to use a skill by name, for example: "Use dumb-down-this-for-me to explain this file." In Codex, you can also use `$dumb-down-this-for-me` or `$tighten-this-pr-for-me`. The slash-command examples above are for Claude Code; invocation syntax varies by agent.

Each agent needs file access for local sources and generated artifacts, and a browser, fetch tool, or connector for URLs. PR access needs `gh` or a GitHub connector. Publishing and video generation still need the tools listed above.

### Claude Code plugin

```
/plugin marketplace add rtio/plainly
/plugin install plainly@plainly
```

Installed as a plugin, the skills are namespaced: `/plainly:dumb-down-this-for-me`. To use the short names, copy a folder from `skills/` into `~/.claude/skills/`.

## Evals

The `evals/` folder has six cases for `claude plugin eval`. Each case runs with and without the plugin, so the report shows what the skills change. These are Claude Code behavioral evals; they do not evaluate other agents.

```
claude plugin eval . --allow-tools Write
claude plugin eval . --tag tighten
```

The level 2 and 3 cases need `--allow-tools Write`. They run without a shell, so they check that the skill writes the files and does not invent a Spacefast link. Level 4 has no eval, because it needs a local voice and minutes of rendering.

## License

MIT. `skills/no-ai-slop` keeps its own MIT license from Peter Yang.
