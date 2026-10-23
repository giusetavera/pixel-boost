<p align="center">
  <img src="assets/img/banner.png" alt="PixelBoost — ChatGPT plugin" width="100%">
</p>

<p align="center">
  <strong>The ChatGPT plugin that turns blurry photos into high-resolution images.</strong><br>
  Upload the photo and PixelBoost does the rest: no prompts to write, no servers, no external services.
</p>

---

## Why

Sharpening a photo with a generative model is easy. Doing it **without changing the photo** is not: you can end up with a different face, a thicker beard or a reinvented background. **PixelBoost** is built to avoid that:

- **Zero prompts.** With the plugin active, just attach an image. It starts on its own, without asking for confirmation or preferences.
- **Faithful to the original.** It enhances only details that are already visible. Identity, expression, composition, colours, lighting and background stay as they are in the uploaded photo.
- **Native to ChatGPT.** It uses the image editing capability built into ChatGPT. No API keys, no MCP, no servers.
- **Honest about its limits.** It never claims dimensions it hasn't verified, and it never passes off a generative reconstruction as a reliable recovery of real detail.

## What it does

PixelBoost is a plugin with a single skill, `pixel-boost`. When you upload an image, it:

1. uses the image itself as the reference for the edit (it doesn't describe it and generate a new one);
2. sends ChatGPT's image tool the **Ultra-Premium 16K Professional Remaster & Enhancement** prompt, negative prompt included, without summarising or rewriting it;
3. requests **a single enhanced version** at the highest quality and resolution supported, keeping the original orientation, aspect ratio and transparency;
4. returns the image directly, with no variants, collages, before/after comparisons or watermarks.

The prompt covers super-resolution, natural sharpening, noise and compression-artifact removal, texture refinement and micro-contrast. The negative prompt explicitly rules out identity or age changes, invented hair and pores, plastic-looking skin, oversharpening, added objects and redesigned backgrounds.

It works best on:

| Image type | What to expect |
|---|---|
| **Blurry or shaky photos** | Sharper edges and more legible detail, without altering the subject or the scene |
| **Low-resolution or grainy photos** | Upscaling with less noise and fewer JPEG artifacts |
| **Scanned old photos** | Cleaner, sharper results that keep the original colours and lighting |
| **Illustrations, screenshots, logos** | More definition in the **original style**: they are not turned into photographs, and existing text and logos stay as they were |

If you attach several images, PixelBoost processes them one at a time, each with its own reference, and never merges them.

## Installation

The repository ships two manifests with the same metadata:

- `plugin.json`: manifest in the [Agent Plugins](https://agent-plugins.org) format, with the `com.openai` extension for ChatGPT;
- `.codex-plugin/plugin.json`: manifest for Codex / ChatGPT, pointing to the `skills/` folder.

Clone the repository:

```bash
git clone https://gitlab.com/giusetavera/pixel-boost.git
```

or build a clean ZIP of the plugin (the README banner is left out of the archive):

```bash
git archive --format=zip --prefix=pixel-boost/ -o pixel-boost.zip HEAD
```

Then install the folder or the ZIP following the plugin procedure of your ChatGPT / Codex client. Menu names may vary by plan and version.

### Other AI agents

The skill follows the open **Agent Skills** format (`SKILL.md` with YAML front matter), so any compatible agent can load it from `skills/pixel-boost/`. The agent must have a native image editing tool, though: PixelBoost doesn't bring its own.

## Usage

Activate the plugin and **attach the photo**. You don't need to write anything.

If you want to be explicit:

> Make this photo sharp and high resolution.

> Use $pixel-boost on all three attached images.

If the same message asks for something else (for example "describe this photo" or "remove the background"), PixelBoost follows your request instead of running the enhancement.

The skill's instructions and default prompts are in Italian, but you can talk to it in any language.

Tips:

- **Upload the original.** The more pixels and the less compression you start with, the more faithful the result. Avoid screenshots of a photo if you have the file.
- **One photo, one result.** PixelBoost doesn't generate extra "more spectacular" versions on its own. If you don't like the result, ask for another attempt explicitly.

## Repository structure

```
pixel-boost/
├── plugin.json                     # Agent Plugins manifest + com.openai extension
├── .codex-plugin/
│   └── plugin.json                 # Codex / ChatGPT manifest
├── skills/
│   └── pixel-boost/
│       ├── SKILL.md                # operating instructions + prompt passed verbatim
│       └── agents/
│           └── openai.yaml         # display name, description and default prompt
└── assets/
    └── img/
        ├── icon.png                # logo and composer icon (512×512)
        └── banner.png              # README artwork (excluded from the ZIP)
```

**`SKILL.md`** holds all of the plugin's behaviour: when to trigger, how to pick the image, what not to do, and the English prompt passed **word for word** to the image tool. To change the style of the result, edit the prompt at the bottom of that file.

If you change the name, descriptions or version, update **both** manifests: they must stay in sync.

## Limits, honestly

- The final quality depends on the ChatGPT image tool available on your account. "16K" in the prompt is a quality target, not a guarantee of native dimensions: PixelBoost states a resolution only after verifying it in the output.
- Detail that isn't in the photo can't truly be recovered. On heavily degraded images the model **reconstructs**, and a reconstruction can be plausible yet wrong.
- Where authenticity matters (documents, evidence, historical archives, forensic work), treat the result as an enhanced viewing copy, not as the original.
- If native image editing is unavailable or returns an error, PixelBoost says so. It doesn't fake a result or fall back to external services.

## Changelog

**1.0.0**: first release of PixelBoost, successor to the legacy *HD Photo Remaster* plugin (`hd-photo-remaster`). New name, new visual identity, and a skill refocused on turning blurry photos into high-resolution images.

## Author

Developed by **Giuseppe Tavera** · [giuseppetavera.it](https://giuseppetavera.it). Feedback, issues and merge requests are welcome.
