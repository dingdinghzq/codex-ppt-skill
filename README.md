# Codex PPT Skill - Uninterrupted Intake Edition

Generate visually unified, image-based PowerPoint presentations from articles,
reports, papers, notes, outlines, or ideas. Each slide is generated as a
complete 16:9 image and assembled into a `.pptx` file with optional speaker
notes.

> [!IMPORTANT]
> This repository is a modified distribution of the original
> [ningzimu/codex-ppt-skill](https://github.com/ningzimu/codex-ppt-skill)
> project by [ningzimu](https://github.com/ningzimu).
>
> The original author created the core skill, generation workflow, scripts,
> documentation, and visual-style library. This repository preserves that
> attribution and the upstream MIT license. It is not the official upstream
> repository.

## What Changed in This Repository

This version changes the user-interaction workflow so all important choices are
collected once, before slide production starts.

### One five-question intake

The skill asks exactly five numbered questions:

1. **Visual style** - Presents all 10 built-in styles with short descriptions,
   recommends one, and accepts a custom image, PDF, PPT, or PPTX reference.
2. **Deck length** - Offers short (6-8), standard (10-12), detailed (15-20), or
   an exact slide count.
3. **Audience and purpose** - Captures who will view the deck, what they should
   learn or decide, and the presentation setting.
4. **Content scope and required assets** - Captures required topics,
   exclusions, logos, charts, screenshots, figures, and asset mapping.
5. **Language and speaker notes** - Captures the slide language and whether
   notes should be included.

Answers already supplied in the user's request are shown as captured choices or
recommended defaults, so the user can correct everything in one reply.

### No interruptions after intake

After the user answers the intake, accepts the defaults, or says to proceed, the
skill works through the entire production process automatically:

- creates the outline without a separate approval round;
- selects the available image backend without asking for confirmation;
- generates and internally reviews one style-anchor slide;
- generates, records, and checks every remaining slide;
- repairs reasonable generation failures without asking whether to retry;
- writes speaker notes when requested;
- assembles and validates the final PowerPoint.

It does not pause for outline approval, backend approval, sample-slide approval,
worker authorization, retry approval, or final assembly approval. It stops only
when completion is genuinely blocked, such as by unavailable credentials or a
missing required source asset.

### Operational defaults

- Uses 16:9 unless the request requires another aspect ratio.
- Uses the current or source directory when no output location is supplied.
- Uses slide workers when the runtime already permits them; otherwise it
  continues with parent-local generation.
- Keeps backend provenance and slide-job state for reproducible assembly.

## Built-In Visual Styles

The intake presents every style stored in [`references/`](references/):

1. Clean Professional (`清爽专业风`)
2. Creative Magazine (`创意杂志风`)
3. E-Ink Editorial (`电子墨水杂志风`)
4. Data Dashboard (`数据仪表盘风`)
5. Academic Defense (`科研答辩风`)
6. Retro Flat Illustration (`复古扁平插画风`)
7. Hand-Drawn Technical Explainer (`手绘技术解释风`)
8. Hand-Drawn Whiteboard (`手绘白板风`)
9. Warm Handmade (`温暖手工风`)
10. Consulting (`麦肯锡风格`)

## Important Output Characteristic

The resulting slides are full-slide images. They provide strong visual
consistency, but individual textboxes, diagrams, and shapes are not separately
editable in PowerPoint.

## Installation

Ask Codex to install this repository:

```text
Install the codex-ppt skill from:
https://github.com/dingdinghzq/codex-ppt-skill
```

Or clone it directly into the Codex skills directory:

```bash
git clone https://github.com/dingdinghzq/codex-ppt-skill.git \
  ~/.codex/skills/codex-ppt
```

Install the Python dependencies when the CLI/API image-generation fallback is
needed:

```bash
python -m pip install -r ~/.codex/skills/codex-ppt/requirements.txt
```

Restart Codex after installation so the skill is discovered.

## Usage

Example request:

```text
Use the codex-ppt skill to create a presentation about Docker for middle
school students.
```

The skill will present the five-question intake once. Reply with all choices in
one message, or say `use the defaults`. It will then complete the deck without
additional preference or approval questions.

## Repository Structure

```text
codex-ppt-skill/
├── SKILL.md
├── docs/
├── prompts/
├── references/
├── scripts/
└── requirements.txt
```

## Upstream and License

- Original project: [ningzimu/codex-ppt-skill](https://github.com/ningzimu/codex-ppt-skill)
- Original author: [ningzimu](https://github.com/ningzimu)
- Modified repository: [dingdinghzq/codex-ppt-skill](https://github.com/dingdinghzq/codex-ppt-skill)
- License: [MIT](LICENSE)

Please visit the upstream repository for the original project, its broader
documentation, examples, releases, and ongoing development.
