# Astro + Svelte Count-Up (Inline) — Complete Guide

Animate numbers inside a sentence and start the animation only when visible. Supports an optional plus sign, prefix/suffix, accent styling (color/weight/size), and a left→right highlighter sweep that runs in sync with the number.

---

## 1) Enable MDX

```bash
pnpm add @astrojs/mdx
```

In `astro.config.mjs`:

```js
import { defineConfig } from 'astro/config';
import mdx from '@astrojs/mdx';

export default defineConfig({
  integrations: [mdx()],
});
```

Rename any Markdown file that needs inline components to `.mdx` (e.g., `about.mdx`).

---

Try to put `mdx()` last in `integrations`

Order matters. Place the MDX integration at the **end** of the `integrations` array so other plugins can run their transforms first. Some integrations (e.g., `astro-expressive-code`) need to process Markdown/MDX **before** MDX takes over, otherwise styles or components may not render correctly.

```js
// astro.config.mjs
import { defineConfig } from 'astro/config';
import expressiveCode from 'astro-expressive-code';
import mdx from '@astrojs/mdx';

export default defineConfig({
  integrations: [
    expressiveCode(), // run first so it can transform content
    mdx(),            // keep MDX last
  ],
});
```

---

## 2) Create the component: `src/components/CountUp.svelte`

Please download from this Github repo.

---

## 3) Use inside MDX (inline in a sentence)

```mdx
import CountUp from "@components/CountUp.svelte";

We reached{' '}
<strong style="color:#ab8ae3">
  <CountUp
    end={370200}
    format="standard"
    plus={true}
    duration={5000}
    suffix=" plays"
    client:visible
    highlight
  />
</strong>
 this month.
```

---

Frontmatter must be at the very top

Astro only parses frontmatter if it appears **as the first thing in the file**.  
Any character before the opening `---`—including whitespace, comments, or MDX `import` statements—will cause the frontmatter to be ignored, and required schema fields (e.g. `title`, `published`) will appear “missing”.

**Correct:**
```mdx
---
title: Inline Count-Up Numbers in Astro (Svelte + MDX)
published: 2025-08-26
---

import CountUp from "@components/CountUp.svelte";

```

## 4) Use inside an Astro template (non-MDX)

```astro
---
import CountUp from "@components/CountUp.svelte";
---

<p class="mt-3">
  Total signups
  <strong style="color:#ab8ae3; margin-left:.25em">
    <CountUp end={272739} duration={1600} client:visible />
  </strong>
  so far.
</p>
```

---

## 5) Props Reference

| Name | Type | Default | Purpose |
|---|---|---:|---|
| `start` | `number` | `0` | Starting value |
| `end` | `number` | `0` | Target value |
| `duration` | `number` (ms) | `1200` | Duration for number & highlighter |
| `decimals` | `number` | `0` | Decimal places (only for `format="standard"`) |
| `locale` | `string \| undefined` | `undefined` | Number locale (e.g., `"en-US"`, `"zh-CN"`); defaults to browser |
| `format` | `'standard' \| 'compact'` | `'standard'` | Standard digits vs. compact notation |
| `prefix` | `string` | `''` | Text before the number (e.g., `'$'`) |
| `suffix` | `string` | `''` | Text after the number (e.g., `' plays'`) |
| `plus` | `boolean` | `false` | Appends a trailing `+` |
| `color` | `string` | `'#ab8ae3'` | Text color |
| `weight` | `number \| string` | `700` | Font weight |
| `scale` | `number` | `1.12` | Relative font size vs. surrounding text |
| `highlight` | `boolean` | `false` | Enables the highlighter sweep |
| `highlightColor` | `string` | `rgba(171,138,227,.25)` | Highlighter color (with alpha) |
| `highlightRadius` | `string` | `'0.35em'` | Highlighter border-radius |
| `highlightPadY` | `string` | `'0.12em'` | Highlighter vertical inset padding |
| `highlightPadX` | `string` | `'0.18em'` | Highlighter horizontal inset padding |

---

## 6) Recipes

**Standard digits with thousands separators**
```mdx
<strong style="color:#ab8ae3">
  <CountUp end={272739} duration={1600} client:visible />
</strong>
```

**Compact display**
```mdx
<CountUp end={350000} format="compact" plus client:visible />
```

**Prefix, suffix, and plus sign**
```mdx
<CountUp prefix="$" end={1999} suffix=" revenue" plus client:visible />
```

**With decimals and locale**
```mdx
<CountUp end={12345.678} decimals={2} locale="en-US" client:visible />
```

**Custom accent styling**
```mdx
<CountUp end={8888} color="#ff7a00" weight={800} scale={1.2} client:visible />
```

**Custom highlighter color & radius**
```mdx
<CountUp
  end={1024}
  highlight
  highlightColor="rgba(255, 246, 117, .45)"
  highlightRadius="0.5em"
  client:visible
/>
```
