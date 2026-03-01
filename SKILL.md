---
name: note-to-blog
description: >
  Transforms a raw note, brain dump, or transcript into a polished magazine-style blog post
  and publishes it directly to a WordPress site. Use this skill whenever the user wants to
  turn rough notes or ideas into a published blog post — even if they say "turn this into a
  post", "write this up for my blog", "post this to my site", or "publish this note". Also
  triggers when the user provides a note and a WordPress URL together, or when they mention
  puttheplayerfirst.com. Use this skill even if the user only provides a raw note and implies
  they want it published — don't wait for them to use the word "skill".
---

# Note to Blog Post

This skill takes a raw note from the user, rewrites it into a polished longform blog post
using a style prompt, and publishes it to their WordPress site via the browser.

## Inputs to collect

Before doing anything, make sure you have all three of these. If any are missing, ask.

1. **The raw note** — the brain dump, review, transcript, or rough ideas to transform.
2. **The writing prompt** — the persona and style instructions for the rewrite. If the user
   doesn't provide one, use the default prompt in `references/default-writing-prompt.md`.
   Always confirm with the user before using the default.
3. **The WordPress URL** — the site to post to (e.g. `puttheplayerfirst.com`). You'll
   navigate to `<url>/wp-admin/post-new.php`.

## Step 1 — Write the post

Read `references/default-writing-prompt.md` if you're using the default writing prompt (or
the user's custom prompt if they provided one).

Follow the prompt's instructions carefully. At minimum this means:

- **Run the Breakdown first** (Overview, Core Structure, Thematic Insights, Depth Map,
  Peak Highlights). This is internal scaffolding — it shapes the post but does not appear
  in the final output published to WordPress.
- **Extract the peak highlight** and use it to open the article.
- **Write the full post** following the style rules (1,200–1,800 words, Esquire/Wired voice,
  first person, editorial section headings, bold opening sentences per paragraph).

Save the complete post — both the Breakdown and the final article — to a markdown file at
`/sessions/great-serene-davinci/mnt/outputs/blog-post.md` and present it to the user with
`present_files` if available.

## Step 2 — Log into WordPress

Navigate to `<wordpress-url>/wp-admin/post-new.php` in the browser.

If the user is not logged in, you will land on the WordPress login page. **Do not enter
the password yourself.** Tell the user you need them to log in, then wait for them to
confirm they've done so before continuing.

## Step 3 — Inject the post content

Once the Gutenberg editor is open, use JavaScript to populate the title and all content
blocks at once. This is faster and more reliable than typing.

```javascript
// Set the title
wp.data.dispatch('core/editor').editPost({ title: "YOUR TITLE HERE" });

// Parse and insert the HTML body
const blocks = wp.blocks.parse(`YOUR HTML CONTENT HERE`);
wp.data.dispatch('core/block-editor').resetBlocks(blocks);
```

Convert the markdown post body to HTML before injecting:
- `## Heading` → `<h2>Heading</h2>`
- `**bold**` → `<strong>bold</strong>`
- `*italic*` → `<em>italic</em>`
- Paragraphs → `<p>...</p>`

Take a screenshot after injection to confirm the title and content are visible in the editor.

## Step 4 — Confirm before publishing

**Always ask the user for explicit confirmation before clicking Publish.** Show them the
post title and confirm they are happy with it. Something like:

> "The post looks good in the editor — title is '[TITLE]', [N] words. Ready to publish?"

Only click the Publish button after they say yes.

## Step 5 — Confirm publication

After clicking Publish, WordPress shows a "Are you ready to publish?" panel. Click the
Publish button in that panel to confirm. Take a screenshot to verify the "is now live"
confirmation appears. Share the live post URL with the user.

## Notes

- The WordPress block editor uses the Gutenberg API (`wp.data`, `wp.blocks`). This works on
  any self-hosted WordPress site running Gutenberg (WordPress 5.0+).
- If the JavaScript injection fails (e.g. `wp` is not defined), fall back to switching to
  the Code Editor view (⋮ → Code editor) and pasting raw HTML directly.
- The Breakdown section is for internal reasoning only — never paste it into the WordPress
  editor.
