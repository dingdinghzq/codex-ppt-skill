# Backend Selection

Read this before selecting the image backend or generating the style-anchor slide.

This skill supports two image backends:

1. Built-in image tool, preferred when available. Example tool names: Codex `image_gen`; OpenClaw `image_generate`.
2. Local API/CLI fallback, using `scripts/image_gen.py`.

## Decision Rules

- Before recommending CLI/API fallback, actively check whether the built-in image generation tool is callable in the current environment. Do not infer availability only from the agent name or subscription context.
- Prefer the built-in image tool when available. In Codex, this usually means the built-in `image_gen` tool. In OpenClaw, this may be `image_generate`. Resolution, quality, aspect ratio, slide-edit requests, or the user saying "use `gpt-image-2`" do not require CLI/API fallback.
- In Codex, treat the built-in image tool as the preferred `gpt-image-2` path when it is available. If the user has a GPT subscription / Codex environment and asks for `gpt-image-2`, do not switch to `scripts/image_gen.py` only to satisfy the model name.
- Use CLI/API fallback only when the built-in tool is unavailable, the built-in tool failed for a required capability, the user explicitly asks for API/CLI or a third-party OpenAI-compatible proxy, or the requested capability is unavailable in the built-in tool.
- Do not recommend CLI/API fallback merely because it provides direct `--out` file paths, easier local file management, local config reuse, batch generation convenience, or simpler automation.
- Before generating the first image, check tool availability and select the backend. State the choice in a progress update, not a confirmation question. Do not treat being in a specific agent environment as proof that the built-in image tool is available.
- CLI/API fallback loads `~/.codex-ppt-skill/.env` automatically. Run the CLI normally; do not manually parse `.env` or ask for configuration before an error.
- Ask for `OPENAI_API_KEY` configuration only after you have intentionally selected CLI/API fallback and that fallback reports missing config, after authentication/base URL/model errors, or when the user explicitly wants to change API settings. Do not mention missing `OPENAI_API_KEY` while the Codex built-in image tool is available. Configure provided values with `scripts/codex_ppt_runtime.py config --api-key`.

If CLI/API fallback is selected, read `cli-api-fallback.md` before generating images. For API key, base URL, model, and `.env` configuration, read `image-model-configuration.md` only after the fallback CLI reports missing or invalid configuration, or when the user explicitly wants to change those settings.

## Progress Update Text

Built-in backend:

```text
我检查到当前环境可调用内置图片生成工具（Codex 通常是 image_gen，OpenClaw 通常是 image_generate），因此将使用内置工具生成风格锚点并继续完成整套幻灯片。
```

CLI/API fallback:

```text
我检查后没有可用的内置图片生成工具，或内置工具缺少必需能力，因此将使用本地 API/CLI fallback 生成风格锚点并继续完成整套幻灯片，读取 ~/.codex-ppt-skill/.env 中的 OPENAI_BASE_URL / CODEX_PPT_IMAGE_MODEL 配置。
```

Do not wait for confirmation. If the user explicitly specified a backend during upfront intake, honor it when available. If the selected backend becomes unavailable after reasonable retries, report a blocker without asking a new question.
