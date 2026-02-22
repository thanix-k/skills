---
name: tailwindcss-expert
description: Enforce Tailwind CSS best practices across every component you touch. Apply these rules automatically whenever writing or reviewing Tailwind class lists, component variants, design tokens, or layout systems.
license: MIT
metadata:
  author: thanix-k
  version: "1.0.0"
---
# Tailwind CSS Expert

Enforce Tailwind CSS best practices across every component you touch. Apply these rules automatically whenever writing or reviewing Tailwind class lists, component variants, design tokens, or layout systems.

---

## DX Enforcement Rules

- All class lists must be **merge-safe** — never concatenate raw strings; always resolve conflicts before they reach the DOM
- Use `cn()` (backed by `clsx` + `tailwind-merge`) for every dynamic or conditional class list
- Avoid **duplicate utilities** in a single class string (e.g., two `p-*` values)
- Keep components **override-safe**: accept a `className` prop and merge it last via `cn()`
- Design tokens must be **tree-shakable** — prefer CSS variables from the theme over one-off arbitrary values

```ts
// Correct
import { cn } from "@/lib/utils";

function Card({ className, ...props }: React.HTMLAttributes<HTMLDivElement>) {
  return (
    <div className={cn("rounded-lg border bg-card p-4 shadow-sm", className)} {...props} />
  );
}
```

---

## Anti-Patterns — Never Do These

| ❌ Pattern | Why it's wrong |
|-----------|----------------|
| `bg-blue-500` | Hardcoded color, bypasses design tokens |
| `text-red-600` | Same — use semantic tokens (`text-destructive`) |
| `w-[342px]` | Magic number, breaks layout flexibility |
| Hardcoded hex in `style={{}}` | Untrackable, not purgeable |
| `mt-[13px]` hacks | Breaks spacing rhythm |
| Stacking many plugins | Increases build complexity with diminishing returns |
| `theme()` calls inside arbitrary values | Bypasses the token system |

---

## Responsive & Layout System

- Abstract complex grids into a **Grid component** using [CVA](https://cva.style):

```ts
import { cva } from "class-variance-authority";

const grid = cva("grid", {
  variants: {
    cols: {
      1: "grid-cols-1",
      2: "grid-cols-2",
      3: "grid-cols-3",
    },
    gap: {
      sm: "gap-2",
      md: "gap-4",
      lg: "gap-8",
    },
  },
  defaultVariants: { cols: 1, gap: "md" },
});
```

- Create a **Container component** that enforces max-width and horizontal padding consistently
- Enforce a **spacing rhythm** — stick to the 4px base scale (`space-1` = 4px, `space-2` = 8px, etc.)
- Prefer **container queries** (`@container`) over viewport breakpoints for component-level responsiveness
- Use the `size-*` shorthand (`size-4` instead of `w-4 h-4`) wherever both dimensions are equal

---

## Accessibility Enforcement

Every interactive component must include:

- **`focus-visible`** ring styles (never use bare `focus:` — it triggers on mouse click):

```html
<button class="focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-[--color-ring]">
```

- **Ring tokens** — define `--color-ring` in your CSS theme, never hardcode ring colors
- **`aria-invalid`** on form fields with validation errors, paired with appropriate `ring-destructive` style
- **`aria-describedby`** linking inputs to their error/hint messages
- **Keyboard navigability** — all interactive elements must be reachable and operable via keyboard
- **Reduced motion support** — wrap animations in `motion-safe:`:

```html
<div class="motion-safe:transition-all motion-safe:duration-200">
```

---

## Quick Checklist

Before finishing any component:

- [ ] All class lists go through `cn()`
- [ ] No hardcoded colors or arbitrary pixel values
- [ ] `className` prop accepted and merged last
- [ ] `focus-visible` styles present on all interactive elements
- [ ] `aria-*` attributes set where required
- [ ] `motion-safe:` wrapping on all transitions/animations
- [ ] Spacing uses the 4px rhythm scale
- [ ] Container queries used instead of breakpoints where appropriate
