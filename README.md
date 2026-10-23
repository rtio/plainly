# plainly

Claude Code skills for making technical text easier to read, with evals.

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

```
/plugin marketplace add rtio/plainly
/plugin install plainly@plainly
```

Installed as a plugin, the skills are namespaced: `/plainly:dumb-down-this-for-me`. To use the short names, copy a folder from `skills/` into `~/.claude/skills/`.

## Evals

The `evals/` folder has six cases for `claude plugin eval`. Each case runs with and without the plugin, so the report shows what the skills change.

```
claude plugin eval . --allow-tools Write
claude plugin eval . --tag tighten
```

The level 2 and 3 cases need `--allow-tools Write`. They run without a shell, so they check that the skill writes the files and does not invent a Spacefast link. Level 4 has no eval, because it needs a local voice and minutes of rendering.

## License

MIT. `skills/no-ai-slop` keeps its own MIT license from Peter Yang.
