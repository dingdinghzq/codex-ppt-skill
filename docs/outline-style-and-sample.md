# Outline, Style, And Style Anchor

Read this before writing or updating `outline.md`, choosing a visual style, using files from `references/`, or generating the style-anchor slide.

If the user asks to save a finished deck style or a user-supplied image/PDF/PPT/PPTX style for future reuse, read `style-library.md`.

## Plan The Deck Outline

Create a concise `outline.md` draft before generating images. For each slide, define:

- Slide number
- Slide title
- 3-5 key points
- Optional visual idea
- Layout role and intent, such as cover, agenda, section divider, concept explanation, process, comparison, timeline, data evidence, architecture, case study, summary, or Q&A
- Required source images, if any, including the image path or attachment name, its role on the slide, and whether it is a strict input asset or only a style/layout reference

Save the final outline to `{base_dir}/{deck_name}/outline.md` once the project directory is known. Build it from the upfront intake and inferred defaults. Do not stop to ask for outline approval after production begins.

If slides use required source images, resolve their mapping during the upfront intake. Record exact paths and roles in `outline.md`, then continue without another mapping confirmation.

After generating and internally accepting a style-anchor slide, record its `slide_XX.png` path as the deck-level style reference. Later slide prompts and worker handoffs should include it as a style-only reference so each page keeps the same palette, typography mood, density, texture, and visual identity without copying the anchor's exact layout.

Recommended structure:

```text
Slide 1: Cover
Slide 2: Context / problem
Slide 3-7: Main argument or sections
Slide 8: Summary / recommendation / closing
```

For slides that use source images, add lines like:

```markdown
Slide 5: Experiment Results
- Key points: ...
- Required images:
  - Main evidence figure; strict input asset; preserve data, axes, labels, legends, colors, and values

    ![Result 01](assets/figures/result_01.png)

  - Supporting model architecture; strict input asset; preserve labels and arrows

    ![Model Architecture](assets/figures/model_architecture.png)
```

Use Markdown image syntax inside the `Required images` list whenever the asset is local and renderable in the outline. This lets the user visually verify the exact asset mapping during outline review. Keep the descriptive text next to each image so `prepare_slide_prompts.py` can convert the same asset into structured prompt input later.

## Choose A Unified Visual Style

Resolve visual style in the required five-question upfront intake. Once production begins, do not open a separate style-selection round.

If the user has already specified a style, provided a style image, or provided a PDF/PPT/PPTX to use as style reference, show that choice as captured in question 1. Briefly state that the custom reference overrides the built-in menu while still making the 10 built-in alternatives visible. After the intake reply, extract the usable style rules and proceed.

For PDF/PPT/PPTX style references, do not infer the visual system from document structure, outline text, XML, file metadata, or slide object hierarchy alone. First render or export representative pages/slides into real page images, inspect those rendered images, and derive the style from what is actually visible on the pages. If the file has multiple visual sections, inspect enough representative pages to capture the shared style and any section-specific variations.

When extracting style from reference material, separate content reuse from style reuse. Unless the user explicitly asks to reuse the source content, treat the provided image/PDF/PPT/PPTX as a style reference only.

Question 1 must list all 10 built-in styles from the `Available references` section, each with a concise audience-facing description. Mark the best fit as recommended. The user may choose one of the 10, supply a custom visual reference, or say to use the recommendation.

If the user explicitly proceeds without choosing, use the recommended option. Create one final style direction and keep the visual identity consistent across all slide prompts. Keep color palette, typography, texture, icon/illustration language, and overall mood stable. Do not reuse the same layout on every page.

The `references/` directory contains optional style references. Use them as inspiration, not as rigid templates. Adapt the style to the topic and audience.

Important: a deck should have one coherent visual identity, not one repeated composition. Treat each reference as a style system: stable palette, typography, icon language, texture, and visual mood; variable page layout chosen from the slide's content role. `layout_blueprints` are candidate starting points only. Do not apply the same blueprint to every slide.

Available references:

- `references/清爽专业风.md`
- `references/创意杂志风.md`
- `references/电子墨水杂志风.md`
- `references/数据仪表盘风.md`
- `references/科研答辩风.md`
- `references/复古扁平插画风.md`
- `references/手绘技术解释风.md`
- `references/手绘白板风.md`
- `references/温暖手工风.md`
- `references/麦肯锡风格.md`

When adding a reusable style to the library, also add its `references/{style_name}.md` file to this list.

Example visual-style question:

```text
1. 请选择视觉风格。我建议使用“清爽专业风”，因为它最适合当前受众和表达目标。
   1) 清爽专业风（推荐）- 浅色、清晰、结构化，适合技术分享和项目汇报。
   2) 创意杂志风 - 大标题、不对称编辑布局，适合创意和品牌叙事。
   3) 电子墨水杂志风 - 高级杂志节奏和克制墨色，适合舞台演讲和观点表达。
   4) 数据仪表盘风 - KPI 卡片和图表网格，适合数据密集型报告。
   5) 科研答辩风 - 正式学术布局和证据结构，适合研究与论文答辩。
   6) 复古扁平插画风 - 复古色彩和友好插画，适合教育、文化和故事主题。
   7) 手绘技术解释风 - 留白充足的技术草图，适合降低复杂概念的理解门槛。
   8) 手绘白板风 - 白板笔记、箭头和教学氛围，适合培训和工作坊。
   9) 温暖手工风 - 纸张拼贴和柔和色彩，适合教育、社区和人文主题。
   10) 麦肯锡风格 - 咨询框架和高管级结构，适合战略、分析和建议。
   也可以提供自定义图片、PDF、PPT 或 PPTX 作为风格参考。
```

## Generate One Internal Style Anchor

After the outline, style, and image backend are selected, generate exactly one style-anchor slide before the remaining pages.

Style-anchor requirements:

- Use the selected style description.
- Prefer a representative content slide over the cover when possible.
- Demonstrate the intended deck rhythm: the anchor should show how the chosen style adapts to a real content page, not just a generic fixed template.
- Save it directly as the intended final slide filename, such as `{base_dir}/{deck_name}/origin_image/slide_08.png`. In CLI/API fallback mode, use `scripts/image_gen.py generate --out` for that exact path.
- Inspect the anchor internally for visual style, typography, layout density, language quality, outline match, and safe margins.
- Repair or regenerate the same slide until it is suitable as the deck-wide reference.
- Mark it accepted in run state and continue directly to full production.

Do not ask the user to approve the anchor. Keep the accepted anchor as the final slide for its page. Do not create `sample_slide.png` in `origin_image/`, because the assembly step is designed around final `slide_XX` filenames.

After the style anchor passes internal QA, record its generation method in `deck_spec.json` before preparing full-deck jobs. The legacy key `sample_generation_method` remains for script compatibility. This is the contract the parent passes to workers so they use the same image-generation path as the anchor, not a cheaper local rendering path. Include at least:

- `backend_used`: the selected backend label, such as `built-in image tool` or `scripts/image_gen.py`.
- `tool_name`: the actual tool or command used, such as `image_gen`, `image_generate`, or `scripts/image_gen.py`.
- `mode`: `generate` or `edit`.
- `prompt_source`: where the accepted anchor prompt came from.
- `size`, `quality`, and model/config details when the backend exposes them.
- `approved_sample_path`: the internally accepted anchor `origin_image/slide_XX.png` path; retain this legacy key for compatibility.
- `input_context_preparation`: how local source/style images were made available, such as `view_image` for built-in mode.
- `handoff_rule`: subagents must use the same backend/tool/mode and return a blocker if that path is unavailable.
