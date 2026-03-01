# Note to Blog

A skill for [Claude Cowork](https://claude.ai) that transforms a raw note, brain dump, or transcript into a polished magazine-style blog post and publishes it directly to a WordPress site.

## What it does

1. Takes your rough notes or brain dump
2. Rewrites them into a 1,200–1,800 word Esquire/Wired-style article
3. Logs into your WordPress site and publishes the post via the Gutenberg editor

## Installation

Install the `note-to-blog.skill` file in Claude Cowork.

## Usage

Just paste your raw notes and say something like:
- "Turn this into a post"
- "Write this up for my blog"
- "Post this to puttheplayerfirst.com"

Claude will handle the rest — drafting, editing, and publishing.

## Customisation

This skill was built for [PutThePlayerFirst.com](https://puttheplayerfirst.com) — a studio focused on serious games and learning design. The writing style, voice, and WordPress URL are all specific to that context.

If you're using this for your own blog, you'll want to customise:

- **`references/default-writing-prompt.md`** — the most important file to change. Update the persona, tone, industry context, and any style rules to match your voice and audience.
- **WordPress URL** — the skill will ask for this each time, but if you always post to the same site, you can hardcode it in `SKILL.md`.
- **Word count and structure** — adjust the target length and narrative arc in the writing prompt to suit your content type (e.g. technical tutorials, thought leadership, product updates).

The skill itself is designed to be general-purpose — the writing prompt is where all the personality lives.
