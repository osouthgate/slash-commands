---
description: Review UI code against Vercel's Web Interface Guidelines
argument-hint: TARGET=<file-or-directory-to-review>
---

<target>
$TARGET
</target>

<context>
First, identify the project's styling setup:
- Check for `tailwind.config.js`/`tailwind.config.ts`, theme files, CSS variables in `:root`/`globals.css`
- Identify dark mode strategy (`class` vs `media`, theme provider, `next-themes`, etc.)
- Note design tokens, colour scales, spacing scales in use
</context>

Tasks:
1) Review against the Vercel Web Interface Guidelines below.
2) Check styling compliance (no inline styles, use design tokens).
3) Check dark mode compliance if applicable.
4) Flag accessibility violations.

<guidelines>

## Interactions

- Keyboard works everywhere: All flows keyboard-operable, follow WAI-ARIA Authoring Patterns
- Clear focus: Every focusable element shows visible focus ring. Prefer `:focus-visible` over `:focus`. Set `:focus-within` for grouped controls
- Manage focus: Use focus traps, move & return focus per WAI-ARIA Patterns
- Match visual & hit targets: If visual target < 24px, expand hit target to ≥ 24px. Mobile minimum 44px
- Mobile input size: `<input>` font size ≥ 16px on mobile to prevent iOS Safari auto-zoom. Or set `<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1" />`
- Respect zoom: Never disable browser zoom
- Hydration-safe inputs: Inputs must not lose focus or value after hydration
- Don't block paste: Never disable paste in `<input>` or `<textarea>`
- Loading buttons: Show loading indicator & keep original label
- Minimum loading-state duration: Add show-delay (~150–300ms) & minimum visible time (~300–500ms) to avoid flicker. `<Suspense>` does this automatically
- URL as state: Persist state in URL so share, refresh, Back/Forward work (e.g., nuqs)
- Optimistic updates: Update UI immediately when success likely; reconcile on server response. On failure, show error & roll back or provide Undo
- Ellipsis for further input & loading states: Menu options opening follow-up end with "…" (e.g., "Rename…", "Loading…", "Saving…")
- Confirm destructive actions: Require confirmation or provide Undo with safe window
- Prevent double-tap zoom on controls: Set `touch-action: manipulation`
- Tap highlight follows design: Set `webkit-tap-highlight-color`
- Design forgiving interactions: Generous hit targets, clear affordances, predictable interactions, prediction cones
- Tooltip timing: Delay first tooltip in group; subsequent peers have no delay
- Overscroll behavior: Set `overscroll-behavior: contain` in modals/drawers
- Scroll positions persist: Back/Forward restores prior scroll
- Autofocus for speed: On desktop with single primary input, autofocus. Rarely on mobile (keyboard causes layout shift)
- No dead zones: If part of control looks interactive, it should be interactive
- Deep-link everything: Filters, tabs, pagination, expanded panels — anytime `useState` is used
- Clean drag interactions: Disable text selection & apply `inert` while dragging
- Links are links: Use `<a>` or `<Link>` for navigation. Never `<button>` or `<div>` for navigational links
- Announce async updates: Use polite `aria-live` for toasts & inline validation
- Locale-aware keyboard shortcuts: Internationalise for non-QWERTY layouts. Show platform-specific symbols

## Animations

- Honor `prefers-reduced-motion`: Provide reduced-motion variant
- Implementation preference: CSS > Web Animations API > JS libraries (e.g., motion)
- Compositor-friendly: Prioritise GPU-accelerated properties (`transform`, `opacity`). Avoid reflow triggers (`width`, `height`, `top`, `left`)
- Necessity check: Only animate when it clarifies cause & effect or adds deliberate delight
- Easing fits the subject: Choose easing based on what changes (size, distance, trigger)
- Interruptible: Animations cancelable by user input
- Input-driven: Avoid autoplay; animate in response to actions
- Correct transform origin: Anchor motion to where it "physically" starts
- Never `transition: all`: Explicitly list only properties you intend to animate (typically `opacity`, `transform`)
- Cross-browser SVG transforms: Apply CSS transforms to `<g>` wrappers. Set `transform-box: fill-box; transform-origin: center;`

## Layout

- Optical alignment: Adjust ±1px when perception beats geometry
- Deliberate alignment: Every element aligns intentionally — grid, baseline, edge, or optical centre. No accidental positioning
- Balance contrast in lockups: When text & icons side by side, adjust weight/size/spacing/colour so they don't clash
- Responsive coverage: Verify on mobile, laptop, & ultra-wide. Zoom out to 50% to simulate ultra-wide
- Respect safe areas: Account for notches & insets with safe-area CSS variables
- No excessive scrollbars: Only render useful scrollbars; fix overflow issues. On macOS set "Show scroll bars" to "Always" to test Windows view
- Let the browser size things: Prefer flex/grid/intrinsic layout over measuring in JS. Avoid layout thrash

## Content

- Inline help first: Prefer inline explanations; tooltips as last resort
- Stable skeletons: Skeletons mirror final content exactly to avoid layout shift
- Accurate page titles: `<title>` reflects current context
- No dead ends: Every screen offers next step or recovery path
- All states designed: Empty, sparse, dense, & error states
- Typographic quotes: Prefer curly quotes (" ") over straight quotes (" ")
- Avoid widows/orphans: Tidy rag & line breaks
- Tabular numbers for comparisons: Use `font-variant-numeric: tabular-nums` or monospace like Geist Mono
- Redundant status cues: Don't rely on colour alone; include text labels
- Icons have labels: Convey same meaning with text for non-sighted users
- Don't ship the schema: Accessible names/labels exist for assistive tech even if not visible
- Use the ellipsis character: `…` over three periods `...`
- Anchored headings: Set `scroll-margin-top` for headers when linking to sections
- Resilient to user-generated content: Layouts handle short, average, & very long content
- Locale-aware formats: Format dates, times, numbers, delimiters, currencies for user's locale
- Prefer language settings over location: Detect via `Accept-Language` header & `navigator.languages`. Never rely on IP/GPS for language
- Accessible content: Set accurate `aria-label`, hide decoration with `aria-hidden`, verify in accessibility tree
- Icon-only buttons are named: Provide descriptive `aria-label`
- Semantics before ARIA: Prefer native elements (`button`, `a`, `label`, `table`) before `aria-*`
- Headings & skip link: Hierarchical `<h1–h6>` & "Skip to content" link
- Brand resources from logo: Right-click nav logo to surface brand assets
- Non-breaking spaces for glued terms: Use `&nbsp;` to keep units/shortcuts/names together: `10&nbsp;MB`, `⌘&nbsp;+&nbsp;K`

## Forms

- Enter submits: When text input focused, Enter submits if only control. If many controls, apply to last
- Textarea behavior: ⌘/⌃+Enter submits; Enter inserts new line
- Labels everywhere: Every control has `<label>` or is associated with one for assistive tech
- Label activation: Clicking `<label>` focuses associated control
- Submission rule: Keep submit enabled until submission starts; then disable, show spinner, include idempotency key
- Don't block typing: Allow any input, show validation feedback. Blocking keystrokes is confusing
- Don't pre-disable submit: Allow submitting incomplete forms to surface validation feedback
- No dead zones on controls: Checkboxes & radios: label & control share single generous hit target
- Error placement: Show errors next to fields; on submit, focus first error
- Autocomplete & names: Set `autocomplete` & meaningful `name` values to enable autofill
- Spellcheck selectively: Disable for emails, codes, usernames
- Correct types & input modes: Use right `type` & `inputmode` for better keyboards & validation
- Placeholders signal emptiness: End with ellipsis
- Placeholder value: Set to example value or pattern (e.g., `+1 (123) 456-7890`, `sk-012345679…`)
- Unsaved changes: Warn before navigation when data could be lost
- Password managers & 2FA: Ensure compatibility, allow pasting one-time codes
- Don't trigger password managers for non-auth fields: Use `autocomplete="off"` or specific token like `autocomplete="one-time-code"`
- Text replacements & expansions: Trim input values to avoid trailing whitespace errors
- Windows `<select>` background: Set `background-color` & `color` on `<select>` to avoid dark-mode contrast bugs

## Performance

- Device/browser matrix: Test iOS Low Power Mode & macOS Safari
- Measure reliably: Disable extensions that add overhead
- Track re-renders: Minimise & make fast. Use React DevTools or React Scan
- Throttle when profiling: Test with CPU & network throttling
- Minimise layout work: Batch reads/writes; avoid unnecessary reflows/repaints
- Network latency budgets: POST/PATCH/DELETE complete in <500ms
- Keystroke cost: Prefer uncontrolled inputs; make controlled loops cheap
- Large lists: Virtualise (e.g., virtua) or use `content-visibility: auto`
- Preload wisely: Preload only above-the-fold images; lazy-load rest
- No image-caused CLS: Set explicit image dimensions & reserve space
- Preconnect to origins: Use `<link rel="preconnect">` for asset/CDN domains (with crossorigin when needed)
- Preload fonts: For critical text to avoid flash & layout shift
- Subset fonts: Ship only code points/scripts you use via unicode-range
- Don't use main thread for expensive work: Move long tasks to Web Workers

## Design

- Layered shadows: Mimic ambient + direct light with at least two layers
- Crisp borders: Combine borders & shadows; semi-transparent borders improve edge clarity
- Nested radii: Child radius ≤ parent radius & concentric so curves align
- Hue consistency: On non-neutral backgrounds, tint borders/shadows/text toward same hue
- Accessible charts: Use colour-blind-friendly palettes
- Minimum contrast: Prefer APCA over WCAG 2 for more accurate perceptual contrast
- Interactions increase contrast: `:hover`, `:active`, `:focus` have more contrast than rest state
- Browser UI matches background: Set `<meta name="theme-color" content="#000000">`
- Set appropriate color-scheme: Style `<html>` with `color-scheme: dark` in dark themes for proper scrollbar/UI contrast
- Text anti-aliasing & transforms: Scaling text can change smoothing. Animate wrapper not text node. Use `translateZ(0)` or `will-change: transform` if artifacts
- Avoid gradient banding: Use masks instead for problematic colour combinations

## Styling & Theming

- No inline styles: Never use `style={{}}` props or `style=""` attributes. Use project's styling system (Tailwind, CSS modules, styled-components, etc.)
- Use design tokens: Colours, spacing, typography, shadows, radii reference theme/design system, not hardcoded values
- Consistent colour references: Use semantic tokens (`text-foreground`, `bg-background`) not raw colours (`text-gray-900`, `#000`)
- Spacing from scale: Use project's spacing scale (`p-4`, `gap-6`) not arbitrary values (`p-[13px]`)
- Typography from system: Font sizes/weights/line heights use defined scales
- Shadow tokens: Use defined shadows (`shadow-sm`, `shadow-card`) not custom `box-shadow`
- Border radius tokens: Use defined radii (`rounded-lg`) not arbitrary values

## Dark Mode

- Dark mode implemented: If project supports it, all components must work in both modes
- No hardcoded colours: Never use values that don't adapt (`bg-white` should be `bg-background` or `bg-white dark:bg-gray-900`)
- Semantic colour usage: Use auto-switching colours: `text-foreground`, `bg-card`, `border-input`
- Check all colour properties: Background, text, borders, shadows, SVG fills/strokes, placeholder text
- Sufficient contrast in both modes: Meet APCA standards in light AND dark
- Images and media: Provide dark variants or use CSS filters/mix-blend-mode
- Form controls styled for both: Inputs, selects, checkboxes adapt properly
- Third-party components themed: External UI libraries respect dark mode
- No flash of wrong theme: Theme applied before hydration (`suppressHydrationWarning`, theme script in head)
- System preference respected: Initial theme follows `prefers-color-scheme` unless user has explicit preference

## Tailwind-Specific (if applicable)

- No `style` props: Use Tailwind classes exclusively
- Minimal arbitrary values: Avoid `[value]` syntax; extend theme config instead
- Consistent breakpoints: Use standard (`sm`, `md`, `lg`, `xl`, `2xl`) not arbitrary
- Use `@apply` sparingly: Prefer utilities in markup; `@apply` only for base styles
- Dark mode prefix consistency: All colour utilities have `dark:` counterparts
- Group hover/focus states: Use `group-hover:`, `peer-checked:` correctly
- Animation classes: Use `animate-*` or extend config; avoid inline keyframes

</guidelines>

Output format:
- Summary:
  - <compliance score: X/10>
  - <one-line assessment>
- Critical issues (must fix):
  - <file:line>: <issue> → <fix>
- Major issues (should fix):
  - <file:line>: <issue> → <fix>
- Minor issues (nice to fix):
  - <file:line>: <issue> → <fix>
- Styling violations:
  - <file:line>: <violation> → <fix>
- Dark mode issues:
  - <file:line>: <issue> → <fix>
- Positive observations:
  - <what's done well>
- Recommended actions:
  - <prioritised fixes with effort estimates>