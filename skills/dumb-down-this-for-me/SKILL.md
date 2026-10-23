---
name: dumb-down-this-for-me
description: Use when the user asks to explain, simplify, or "dumb down" an article, URL, doc, thread, or pasted text, including requests for ASD-STE100 or Simplified Technical English, a "% of the way to STE" level, an explanatory diagram, an interactive HTML explainer page, or a 3Blue1Brown-style explainer video.
argument-hint: "<url | file | text> [level: 1 writing | 2 diagram | 3 html | 4 video] [STE percent for level 1, default 80]"
---

# Dumb Down This For Me

Explain a technical source at one of four levels. The levels come from Andrej Karpathy's post of 2 October 2026 (https://x.com/karpathy/status/2105819303471976479). Follow the post as written. Each level is a different output format, and the post calls each one "even better" than the one before. Produce only the level asked for. Do not combine levels.

## Parameters

| Parameter | Values | Default |
|---|---|---|
| Source | URL, file path, or pasted text | The last source in the conversation |
| Level | `1`/`writing`, `2`/`diagram`, `3`/`html`, `4`/`video` | `1` |
| Percent | Level 1 only: how close to full ASD-STE100 | `80` |

## Read the source first

Read the whole source with the tool that reaches it: WebFetch for a public URL, Read for a file, or an MCP or CLI tool for private links. WebFetch paraphrases, so fetch a second time for exact names, numbers, and dates. x.com returns HTTP 402. For x.com, curl `https://api.fxtwitter.com/<user>/status/<id>` and read `.tweet.text` and `.tweet.media`.

State only what the source says. Label anything else as outside knowledge.

## Level 1 - writing

> Ask your LLM to explain something in ASD-STE100, it's a controlled language specification originally developed for aerospace maintenance documentation. [...] Sometimes I've tried to soften it a bit e.g. ask for "80% of the way to ASD-STE100" because the spec is quite stringent.

Write the explanation "<percent>% of the way to ASD-STE100". At 100, apply every rule below. Below 100, soften the rules in proportion. The post gives no finer definition. Return only the explanation, with no note about how you applied the rules.

Rules, from the STE reference sheet attached to the post:

- Procedural sentences: 20 words max. Descriptive sentences: 25 words max.
- Paragraphs: 6 sentences max, one topic each.
- Noun clusters: 3 words max.
- One instruction per sentence, unless the actions are simultaneous.
- Approved verb forms: imperative, simple present, simple past, simple future, infinitive, and past participle used as an adjective. Not approved: -ing forms (except in technical names), perfect tenses, and the passive in procedures. In descriptive text, use the passive only when necessary.
- Use the same word for the same thing every time. One word, one meaning, one part of speech.
- Do not leave out "the", "a", and "this".
- Use vertical lists for complex text.
- Safety instructions: first a clear, simple command, then the risk. WARNING = risk of injury. CAUTION = risk of damage.
- Word swaps: commence → start, ensure → make sure, prior to → before, replenish → fill, utilize → use, approximately → about, in order to → to, close (adj) → near.

## Level 2 - diagram

> Instead of writing, ask your LLM to create a diagram. These can be a lot easier to process, parse, and understand.

Make a diagram instead of prose, with text kept to labels. The image attached to the post is an example of this level: a one-page reference sheet with labeled panels.

Save `diagram.svg` and an `index.html` that shows it at full width (SVG inline) in a new folder named `YYYY-MM-DD-<topic>-diagram/`. Put the folder where the project's instructions say generated files go, or in the scratchpad. Publish the folder to Spacefast (see below) and return the link.

## Level 3 - html

> Ask for output "in HTML" to get a beautiful, interactive webpage. LLMs are getting really good at frontend and can create beautiful experiences, animations, etc.

Build a beautiful, interactive web page that explains the source, with animations where they help. Make it a self-contained `index.html` in a new folder named `YYYY-MM-DD-<topic>-page/`, placed the same way as for level 2. Publish the folder to Spacefast (see below) and return the link.

## Publish to Spacefast (levels 2 and 3)

Each folder becomes one Space. `sf publish` writes `.spacefast/state.json` into the folder, so publishing the same folder again updates the same Space.

1. Run `sf whoami`. If `sf` is missing or reports `auth_required`, stop and tell the user. Do not install or sign in on their behalf.
2. Choose the team: the one the user names, otherwise `$SPACEFAST_TEAM`. If neither is set, ask.
3. Publish: `sf publish <folder> --team <team> --access private --name "<title>" --slug <topic> --json -y`. If the slug is taken, publish again without `--slug`. Read `liveUrl` from the JSON.
4. On the first publish only, create a view link: `sf share link create --space <liveUrl> --name "<title>" --role viewer --json -y --show-secret`. On a republish, the existing link keeps working (it follows `live`); find it with `sf share link ls --space <liveUrl> --json`.
5. Check with `curl -s -o /dev/null -w '%{http_code}'`: the share link returns 200, and the bare live URL does not.
6. Return the share link (for others) and the live URL (for the user, who is signed in). Never return a link that did not come from `sf` output.

## Level 4 - video

> Experiment with things like "Create a 3b1b style video explainer on X. Use my ElevenLabs API key for audio narration". (you'd need an API key for the latter or you can ask your LLM to find you decent free alternatives that use your local compute).

Make a 3Blue1Brown-style explainer video: a dark background, clean visuals that animate and build up step by step, and narration that sets the pace.

Narration:

- If `ELEVENLABS_API_KEY` is set and the source is public, use the ElevenLabs text-to-speech API (`xi-api-key` header; `GET /v1/voices` lists voices).
- Otherwise, use a free local voice. Do the same for private or internal content, which must not leave the machine. Options: Kokoro-82M (`kokoro-onnx`, voice `af_heart`), or on macOS the Siri natural voices through `AVSpeechSynthesizer`.

Pipeline:

1. Build the visuals and captions as HTML with a paused GSAP timeline.
2. Capture frames with Playwright driving a local Chrome.
3. Encode H.264 with ffmpeg. On macOS without ffmpeg, use Swift `AVAssetWriter`.
4. Mux the narration with ffmpeg. On macOS without ffmpeg, convert with `afconvert -d aac@44100` and mux with Swift `AVMutableComposition`.
5. Synthesize the narration line by line and write a cue file, so the timeline follows the audio.

If the project already has an earlier explainer-video folder, copy it as the template. Save the MP4 where the user keeps videos (`~/Movies/` on macOS), and give its path.
