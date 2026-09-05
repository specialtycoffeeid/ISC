# SpecialtyCoffee.Shop — landing page draft

Single self-contained `index.html`. No build step, no dependencies except Google
Fonts. Open it in a browser to view.

Structure adapted from `authenticom.com/product/recordrecharge`; content, palette
and typography are specific to SpecialtyCoffee.Shop.

## Brief as agreed

| Decision | Choice |
|---|---|
| Language | English |
| Primary CTA | Request samples & pricing |
| Audience | B2C primary, with a bulk/wholesale lane for large buyers |
| Visual direction | Brand green, dark cinematic |

## Section map (reference → this page)

| RecordRecharge | This page |
|---|---|
| Hero + email capture | Hero + sample/pricing request |
| "Daniel Morgan" customer card | Example lot card (Bali Kintamani BK-2417) |
| Data quality alerts | Lot verification — 3 checks passed |
| 37.9B records / trust metrics | 16 origins · SCA 85+ · cupped 2× · 1 kg minimum |
| Pain by department (6 roles) | Who it's for (5 buyer types), B2C first |
| 8-service grid | 8 sourcing services |
| "Four systems, four versions of the same customer" (animated convergence) | "Four sellers. Four versions of the same lot." (animated convergence) |
| Three use-case callouts | Three use-case callouts |
| Interactive demo | Origins grid (6 shown, links to shop) |
| Cost calculator (4 role tabs) | Cost calculator (3 buyer tabs) |
| "Changing DMS providers?" 3 steps | "Switching green suppliers?" 3 steps |
| DXM platform block | Bulk & wholesale lane |
| — | FAQ (7 questions, FAQPage schema) |
| Footer | Footer |

## Replace before publishing

- **Example lot card (BK-2417)** — invented values. Labelled on-page as an
  example; swap for a real lot or remove the caption once it is real.
- **The verified record** in the convergence diagram — same caveat.
- **Origin card attributes** (altitude, process, profile) — these are typical
  ranges for each origin, not your lot data. Confirm per lot.
- **"16 origins"** — your site lists 16 countries but the headline says "14+".
  Pick one and use it everywhere.
- **Hero background** — currently CSS gradient + SVG grain. Drop a real origin
  photograph into `.hero__art` (there is a comment marking the spot). Use WebP/AVIF
  with explicit `width`/`height` so it does not shift layout.
- **Form endpoint** — the submit handler is a stub. Wire `#request` to your CRM,
  form service, or WhatsApp Business API. Search for `Wire this to your CRM`.
- **Shop URLs** — all point at `https://specialtycoffee.shop/shop/`. Point each
  origin card at its own category or product page.
- **No testimonials or review counts are included.** Add them only with real,
  attributable quotes — fabricated social proof is both a legal and a trust risk.

## SEO notes

Semantic-first, following the Koray Tuğberk Gübür approach:

- One `<h1>`; 70 headings total, no skipped levels (verified).
- Every section opens with a declarative sentence that answers its own heading,
  so the passage can be lifted as an answer.
- Entities carry explicit attributes in `<dl>` pairs rather than prose only — origin, varietal, process, altitude band, crop year, cup score, moisture.
- FAQ questions are real query-shaped `<h3>`s inside `<summary>`, mirrored in
  `FAQPage` JSON-LD.
- JSON-LD graph: `Organization`, `WebSite`, `WebPage`, `BreadcrumbList`,
  `ItemList` (origins), `FAQPage`.
- Contextual bridges: hero → traceability gap → services → cost → switching,
  so each section earns the next.

## Accessibility & responsive — what was verified

Checked in Chromium at 375 / 768 / 1440 px:

- No horizontal page scroll at any width, including the diagram at 375 px.
- Contrast: body text 17.8:1, muted text 9.1:1, accent 11.1:1, primary button
  9.2:1, control borders 3.4–3.7:1. All above WCAG 2.2 AA.
- All pointer targets ≥ 44 px tall.
- No dangling `aria-controls` / `aria-labelledby`; every input has a `<label>`.
- Accordion uses the ARIA APG pattern (`h3 > button`, `aria-expanded`), one panel
  open at a time.
- Calculator tabs have roving `tabindex` and Arrow/Home/End key support.
- Form validates on blur (not per keystroke), sets `aria-invalid`, shows the error
  next to the field, and moves focus to the failing input.
- `prefers-reduced-motion` disables all motion and zeroes animation delays;
  `color-scheme: dark` makes native selects and scrollbars match.
- Calculator maths verified: 400 kg × 12 × 15% × $11 = $7,920.

## The convergence diagram

The "four sellers" section is an animated SVG diagram, modelled on the
reference's four-systems-into-one-record graphic: four flawed listings on the
left, connectors converging into a coffee-lot stack, one verified record out to
the right.

- **Connector geometry is computed at runtime** from the real bounding boxes of
  the cards, hub and result panel (`layoutWires()`), and recomputed on a
  debounced resize. Edit the card copy or change the column widths and the wires
  stay attached — they are not hardcoded coordinates.
- Static `d` attributes in the markup are the no-JS fallback, correct at ~1440px.
- Animation starts only when the diagram scrolls into view (IntersectionObserver,
  `threshold: 0.25`), and runs once — `unobserve` after firing.
- Below 1024px the layout stacks and the curved wires are replaced by vertical
  pulse connectors above and below the hub.
- Under `prefers-reduced-motion` the looping spark and breathing glow are turned
  off and everything renders in its final state. The global reduce block zeroes
  `animation-delay` as well as duration — without that, a staggered reveal leaves
  content blank for the length of its delay.
- With JavaScript off, nothing is hidden: the `.is-live` class gates the
  animation, not the visibility.

## Known trade-offs

- **Dark only.** No light theme. The palette is defined in `:root` custom
  properties, so a light variant is a token swap, not a rewrite.
- **Google Fonts is render-blocking.** Self-host Barlow / Barlow Condensed
  (latin subset is ~22 KB per weight) if Core Web Vitals matter.
- **The calculator is a persuasion device, not a quote.** Its assumptions are
  disclosed on-page. Keep it that way.
- **The diagram replaced the comparison table**, rather than being added
  alongside it. Running both would state "four sellers, four versions" twice on
  one page — duplicate content for readers and for search engines alike. If you
  want the table back for scanability, it should replace the diagram, not join it.
