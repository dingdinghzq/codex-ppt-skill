# Upfront Intake And Progress

Read this before creating downstream artifacts, advancing between phases, or reporting progress.

## Single Upfront Intake

Collect all material questions before production begins. Use exactly one consolidated five-question intake. Do not split it into outline, style, backend, sample, asset-mapping, or worker-authorization approval rounds.

Before asking questions:

- Inspect the user's request, supplied files, source material, and callable tools.
- Capture answers already present in the request.
- Infer recommended defaults for omissions.
- Present captured choices and defaults inside the five-question intake so the user can correct everything in one reply.

The five required intake questions are:

1. **Visual style**
   - Present all 10 styles from `references/` with one-line descriptions.
   - Mark one style as recommended for the topic and audience.
   - Allow a custom image, PDF, PPT, or PPTX reference.
2. **Deck length**
   - Ask for an exact count or offer `Short: 6-8`, `Standard: 10-12 (recommended)`, and `Detailed: 15-20`.
   - Use the user's time limit to recommend a count when available.
3. **Audience and purpose**
   - Ask who will view the deck, what they should learn or decide, and whether it is a lesson, talk, pitch, report, or defense.
4. **Content scope and required assets**
   - Ask what must be included or excluded and whether supplied files, logos, charts, screenshots, or figures must appear.
   - Resolve required asset mapping here when source assets exist.
5. **Language and speaker notes**
   - Ask for the slide language and whether to include speaker notes.

Do not add a sixth question for backend, aspect ratio, output location, style-anchor approval, or worker authorization. Infer these operational choices from context and available tools. Use 16:9 and the current/source directory by default.

## Built-In Style Menu

Present these 10 styles in the visual-style question. Translate the descriptions into the user's language when useful, but preserve the reference names so the selected file is unambiguous.

1. `清爽专业风` - Clean Professional: light background, crisp hierarchy, structured diagrams; suited to technical talks, reviews, and project reports.
2. `创意杂志风` - Creative Magazine: bold editorial typography, asymmetric composition, strong imagery; suited to brand and creative storytelling.
3. `电子墨水杂志风` - E-Ink Editorial: premium editorial rhythm, serif headlines, restrained ink textures; suited to stage talks, technology launches, and opinion-led narratives.
4. `数据仪表盘风` - Data Dashboard: KPI cards, charts, and analytical grids; suited to metrics, operations, and data-heavy reporting.
5. `科研答辩风` - Academic Defense: formal blue-and-white evidence layouts, technical routes, tables, and conclusions; suited to research and thesis defenses.
6. `复古扁平插画风` - Retro Flat Illustration: cream paper, vintage colors, friendly flat-vector scenes; suited to education, culture, travel, and narrative topics.
7. `手绘技术解释风` - Hand-Drawn Technical Explainer: airy paper, precise sketches, sparse pastel annotations; suited to explaining complex technical ideas simply.
8. `手绘白板风` - Hand-Drawn Whiteboard: marker diagrams, arrows, notes, and teaching energy; suited to workshops, training, and brainstorming.
9. `温暖手工风` - Warm Handmade: paper collage, soft colors, tactile human-centered storytelling; suited to education, community, children, and cultural themes.
10. `麦肯锡风格` - Consulting: restrained executive grid, analytical frameworks, business metaphors, and boardroom-ready hierarchy; suited to strategy and recommendations.

Suggested five-question intake:

```text
Before I start, please answer these five items in one reply. Choices already stated in your request are captured below. You can also say "use the defaults." After this intake, I will complete the deck without further questions.

1. Visual style: <list all 10 built-in styles with one-line descriptions; mark one recommended; allow custom reference>
2. Deck length: Short 6-8 / Standard 10-12 (recommended) / Detailed 15-20 / exact count
3. Audience and purpose: <captured or recommended audience, learning/decision goal, and setting>
4. Content scope and required assets: <captured must-include/exclude points and asset mapping, or "none supplied">
5. Language and speaker notes: <captured or recommended language; notes yes/no>
```

If the user already supplied one or more answers, keep the same five numbered items and label those answers as captured. A normal initial request such as "create a slide deck about X" does not skip the intake. Skip it only when the user explicitly says not to ask questions or to use defaults immediately. After the intake is shown, if the user refuses further clarification, says to use judgment, accepts the defaults, or says to proceed/create, lock the recommendations and start production.

## Uninterrupted Production

Production begins when the user replies to the intake, accepts the recommended defaults, says to use judgment, or explicitly says to proceed/create the deck. The initial request to make a deck is not itself completion of the intake.

After production begins:

- Do not ask for outline approval.
- Do not ask the user to choose or reconfirm a style; use the intake choice or recommendation.
- Do not ask the user to change or reconfirm deck length; use the intake choice or recommendation.
- Do not ask for backend confirmation; select it with `backend-selection.md`.
- Do not show a sample slide for approval. Generate and internally QA one style-anchor slide.
- Do not ask again for required-asset mapping; use the intake mapping or the clearest reasonable mapping.
- Do not ask for parallel-worker permission. Use workers only when the runtime already permits them; otherwise generate parent-locally.
- Do not ask whether to retry or repair a failed slide. Retry reasonable transient failures and repair internally.
- Do not ask whether to assemble after QA. Assemble when completion criteria are met.
- Do not introduce new preference questions during production. Resolve non-blocking ambiguity with the captured answers, recommended defaults, source material, and professional judgment.

Only stop when completion is genuinely impossible, such as a missing strict input asset, unavailable selected backend after retries, authentication failure requiring credentials, or contradictory requirements with no safe interpretation. Report the blocker and evidence directly; do not turn the blocker report into another approval question.

## Visible Progress Plan

For non-trivial decks, keep a user-visible checklist with one active step:

1. Complete upfront intake and prepare source decisions.
2. Generate and internally accept one style-anchor slide.
3. Prepare slide jobs and slide state.
4. Dispatch available slide workers or parent-local jobs.
5. Record generated slide results.
6. QA, repair, notes, and PPT assembly.

Completion evidence:

- `Complete upfront intake and prepare source decisions`: unresolved material questions were answered or defaulted, `outline.md` exists, and the image backend is selected.
- `Generate and internally accept one style-anchor slide`: one final `origin_image/slide_XX.png` passes parent QA and is marked accepted as the style reference.
- `Prepare slide jobs and slide state`: `prompts/slide_XX.json`, `slide_jobs.json`, and `slide_run_state.json` exist.
- `Dispatch available slide workers or parent-local jobs`: each slide is recorded by `record_slide_dispatch.py` with its real worker id or `parent-local`.
- `Record generated slide results`: each worker output is recorded by `record_slide_result.py`, which copies the selected image into `origin_image/slide_XX.png` and records backend provenance.
- `QA, repair, notes, and PPT assembly`: every expected final image exists, QA is complete, `speech.md` is final, and `{deck_name}.pptx` exists.

Do not mark a step complete just because the chat says it is complete; use real files or script-recorded state.
