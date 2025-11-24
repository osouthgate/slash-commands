---
description: Comprehensive SolidJS coding rules and best practices
argument-hint: CODEFILES=<code-files-to-review>
---

<CODEFILES>
$CODEFILES
</CODEFILES>

## 2. Reactivity basics

- Components run once. Treat the component body as setup, not as a render loop. :contentReference[oaicite:0]{index=0}  
- Use:
  - `createSignal` for simple local state.
  - `createStore` or `createMutable` for nested objects. :contentReference[oaicite:1]{index=1}
  - Context for cross-cutting app state.
- No prop destructuring in components. Always keep `props` whole and access with `props.foo`. This keeps reactivity intact. :contentReference[oaicite:2]{index=2}  
  - Allowed: `function Button(props: ButtonProps) { return <button>{props.label}</button>; }`
  - Not allowed: `function Button({ label }: ButtonProps) { ... }`
- Put reactive reads in:
  - JSX
  - `createEffect`, `createMemo`, `createResource`
  - event handlers  
  Avoid reading signals in plain top-level helper functions that run once and never react. :contentReference[oaicite:3]{index=3}  

## 3. Control flow in JSX

- Use Solid control flow helpers:
  - `<Show when={condition()} fallback={...}>`
  - `<For each={items()}>{item => ...}</For>`
  - `<Switch>` / `<Match>` for multi-branch cases
- Avoid `.map()` directly in JSX for main lists. Prefer `<For>` for better tracking and intent. :contentReference[oaicite:4]{index=4}  
- No big `if` statements around the whole JSX block. Keep the component body simple and move branches into JSX helpers.

## 4. Async and side effects

- For async server calls tied to state, use `createResource` and read through the resource in JSX. :contentReference[oaicite:5]{index=5}  
- Network calls, timers, subscriptions, DOM reads:
  - Set up in `createEffect`.
  - Clean up with `onCleanup`.
- Avoid side effects in the component body. The body should only wire signals, resources, and JSX.

## 5. Props and component API

- Components receive a single `props` object. No rest parameters.
- Keep prop count small. If a component has more than 8 props, group related values into a config object or split the component.
- Use clear prop naming with TypeScript types for every public component.
- Optional props must have safe defaults inside the component.

## 6. Styling rules

- Use `class` / `classList` instead of `className`. :contentReference[oaicite:6]{index=6}  
- Keep styling strategy consistent per project (e.g. Tailwind or a Solid-friendly CSS-in-JS). :contentReference[oaicite:7]{index=7}  
- No large inline style objects. If styles grow, move them into CSS modules or a design system.
- Do not mix many styling approaches in the same file.

## 7. File and folder layout

- Feature-based folders, not giant global buckets. For example:

  - `features/todos/components/TodoList.tsx`
  - `features/todos/hooks/useTodos.ts`
  - `features/todos/stores/todoStore.ts`

- Each feature folder should have:
  - small presentational components
  - hooks/stores
  - tests

## 8. Tooling and linting (enforce the rules)

Use ESLint + `eslint-plugin-solid` + Prettier to keep the style consistent. :contentReference[oaicite:8]{index=8}  

Example ESLint snippet:

```js
// eslint.config.mjs (fragment)
import solid from 'eslint-plugin-solid';

export default [
  {
    files: ['src/**/*.{ts,tsx}'],
    plugins: { solid },
    rules: {
      // Solid-specific rules
      'solid/no-destructure': 'error',
      'solid/no-react-deps': 'error',
      'solid/reactivity': 'warn',
      'solid/prefer-for': 'warn',

      // Size and complexity
      'max-lines': ['error', { max: 500, skipBlankLines: true, skipComments: true }],
      'max-lines-per-function': [
        'error',
        { max: 150, skipBlankLines: true, skipComments: true },
      ],
      'max-depth': ['warn', 3],
    },
  },
];
