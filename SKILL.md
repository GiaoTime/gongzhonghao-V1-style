---
name: gongzhonghao-V1-style
description: Use this independent skill only when the user manually invokes gongzhonghao-V1-style, or when the same request contains both a WeChat Official Account publishing/upload intent such as 上传到公众号, 发公众号, 写公众号, 帮我发公众号, 发布公众号, and a V1 style signal such as 使用V1, 用V1, V1. Do not trigger this skill for generic 公众号排版, 微信公众号排版, or other layout/editing requests, even if they mention V1, unless they also include a publish/upload intent. Do not trigger for 写公众号, 发公众号, 发布公众号, 帮我发公众号, or 上传到公众号 by themselves unless V1 is also mentioned. This skill is for WorkBuddy-assisted new WeChat Official Account article uploads using the user's V1 layout standard, and uploaded articles must always be saved as new drafts, never published directly and never modifying or overwriting existing drafts.
---

# 微信公众号排版规则 V1

## Goal

Turn a draft, Markdown article, outline, or existing text into a WeChat Official Account-ready layout that follows the V1 typography and spacing rules.

This skill is about layout discipline, not content expansion. Preserve the user's meaning and information unless they explicitly ask for rewriting, polishing, expansion, or a different editorial style.

## Trigger Rules

Use this skill only in either of these cases:

1. The user manually calls `gongzhonghao-V1-style`.
2. The request contains both:
   - A WeChat Official Account publish/upload intent, such as `上传到公众号`, `发公众号`, `写公众号`, `帮我发公众号`, `发布公众号`.
   - A V1 signal, such as `使用V1`, `用V1`, or `V1`.

Do not use this skill for generic公众号 requests unless they satisfy the dual condition: publish/upload intent plus V1 signal.

Examples that should trigger:

- `使用V1，把这篇文章上传到公众号`
- `帮我发公众号，用V1`
- `发布公众号，V1`
- Manual skill invocation: `gongzhonghao-V1-style`

Examples that should not trigger:

- `上传到公众号`
- `发公众号`
- `写公众号`
- `公众号排版`
- `帮我把文章排成公众号格式`

## WorkBuddy Upload Rules

When WorkBuddy or another automation tool uploads an article to a WeChat Official Account with this skill:

1. Always create a new article draft.
2. Always save the result as a draft.
3. Never publish, mass-send, schedule, or otherwise make the article public.
4. Never open an existing draft for modification as the default path.
5. Never overwrite or reuse existing draft content.
6. If an article with the same title already exists in drafts, still create a new draft unless the user explicitly gives a different instruction.
7. After saving, return the draft status and any fields that need manual confirmation, such as title, cover, author, summary, original/source notes, and image sizing.

## Input Handling

1. Identify the input type: raw text, Markdown, article outline, image plan, or an existing formatted draft.
2. Preserve factual content, sequence, quotes, data, dates, names, and source notes.
3. If the input is too short, create a compact publish-ready structure rather than inventing new arguments.
4. If the user asks only for an audit, return issues and concrete fixes instead of rewriting the whole article.
5. If the user asks for a final version, return a directly copyable version plus a short layout checklist.

## Typography Rules

Apply these defaults unless the user provides a stricter house style.

### Font Family

- Use one sans-serif font family throughout.
- Prefer `微软雅黑` or `苹方 / PingFang SC`.
- Do not mix multiple fonts.
- Do not use 宋体 for the body.

### Font Size Hierarchy

Use a strict hierarchy:

| Element | Size |
| --- | --- |
| Main title | 20px |
| Section title | 18px |
| Body text | 14-16px |
| Notes/captions | 10px |

Rules:

- Keep same-level text at the same size.
- Do not add extra font levels unless the platform template requires them.
- Use weight, spacing, or color restraint for emphasis before adding new sizes.

### Alignment

- Body text should be justified.
- Avoid ragged paragraph blocks when preparing HTML/CSS output.

## Spacing Rules

### Letter Spacing

- Default letter spacing: `1.5px`.
- Long-form articles: `1.6px` to `2px`.
- Never use `0`.
- Never exceed `2px`.

### Line Height

- Recommended line height: `1.75`.
- Acceptable upper limit: `2.0`.
- Avoid values below `1.6` because they feel compressed.
- Avoid values above `2.0` because they feel loose.

### Page Margin

- Left and right content margin: `10px` to `14px`.
- Never make body text touch the edge.
- Avoid margins above `20px`.

## Paragraph Rules

- Do not indent the first line.
- Aim for roughly 20 Chinese characters per visual line where possible.
- Keep each paragraph around 3-4 lines.
- Absolute maximum: 5 lines per paragraph.
- Use about `30px` paragraph spacing.
- Split dense sections with blank lines, images, quote blocks, or compact lists.
- Do not allow long paragraph walls.

When rewriting paragraph structure, change line breaks and rhythm only. Do not add unsupported claims.

## Color Rules

### Body Color

Choose one body gray and use it consistently:

- `#595959`
- `#3f3f3f`
- `#262626`

Do not use pure black for body text.

### Highlight Colors

Allowed highlight colors:

- Red: `#C00000`
- Blue: `#366092`
- Orange: `#F16522`
- Yellow: `#FFC000`

Rules:

- Use no more than 2 highlight colors per screen.
- Use highlights for key words, section cues, or important numbers.
- Avoid high-saturation color clutter.
- Prefer fewer colors and fewer highlighted phrases.

## Image Rules

Use standard image sizes and keep aspect ratios intact.

| Image type | Size |
| --- | --- |
| Cover | 900 x 383 px |
| Secondary article thumbnail | 200 x 200 px |
| Horizontal image | 1024 x 768 px |
| Vertical image | 1280 x 1920 px |
| Square image | 1080 x 1080 px |
| Footer image | 1080 x 280 px |

Rules:

- Keep image proportions consistent.
- Do not stretch or squeeze images.
- When the user provides nonstandard images, flag resize/crop requirements.

## Output Formats

Choose the output format that best matches the user's request.

### For a Final Formatted Draft

Return:

1. `标题`
2. `摘要 / 导语` if useful
3. `正文排版稿`
4. `图片规格建议` if images are involved
5. `发布前检查清单`

The body should be copyable into a WeChat editor. Use plain Markdown when no HTML/CSS is requested. Use simple HTML snippets only when the user asks for exact style parameters.

### For HTML/CSS Style Output

Use restrained inline or class-based CSS:

```html
<section style="font-family: 'PingFang SC', 'Microsoft YaHei', sans-serif; color: #3f3f3f; font-size: 15px; line-height: 1.75; letter-spacing: 1.5px; text-align: justify; padding: 0 12px;">
  ...
</section>
```

Keep CSS simple enough for a WeChat editor workflow. Do not rely on scripts, remote fonts, complex layout systems, or fragile effects.

### For an Audit

Return findings grouped by:

- 字体与字号
- 对齐与间距
- 段落切割
- 颜色与高亮
- 图片规格
- 可直接修改的建议

Prioritize violations of the V1 constraints over subjective taste comments.

## Final Checklist

Before finishing, verify:

- Single font family.
- Main title 20px, section title 18px, body 14-16px, notes 10px.
- Body text uses justified alignment when style output is requested.
- Letter spacing is 1.5-2px.
- Line height is 1.75 unless there is a reason to vary it.
- Left/right margin is 10-14px.
- No first-line indent.
- Paragraphs stay within 5 lines.
- Paragraph spacing is about 30px.
- Body text is one consistent gray, not pure black.
- Highlights use no more than 2 colors per screen.
- Images use standard size/aspect ratio guidance.
- The result remains restrained, readable, and not over-decorated.
