# Progress Tracker

Update this file whenever the current phase, active feature, or implementation state changes.

## Current Phase

- Feature unit: `02-editor` (editor chrome shell — navbar and project sidebar)

## Current Goal

- Build the base Chrome components that frame every editor screen: the top nav bar and the left project sidebar shell, plus confirm the dialog pattern is ready for future use.

## Completed

### `02-editor`

- Created `components/editor/editor-navbar.tsx`: fixed full-width top bar (`h-14`, `bg-surface`, bottom `border-surface-border`), left/center/right sections. Left section holds the sidebar toggle button (`PanelLeftOpen`/`PanelLeftClose` from lucide-react, swapped based on an `isSidebarOpen` prop). Center and right sections are empty placeholders for future content. Controlled via `isSidebarOpen`/`onToggleSidebar` props — no internal state, so the parent page owns sidebar open/closed state.
- Created `components/editor/project-sidebar.tsx`: floating overlay sidebar (`fixed`, positioned under the navbar, `z-30`) that slides in/out from the left via a `translate-x` transition and never pushes page content. Controlled via `isOpen`/`onClose` props. Header shows a "Project" title and a close button. Body uses shadcn `Tabs` with "My Project" and "Shared" tabs, each rendering an empty placeholder state. Footer has a full-width `New Project` button with a `Plus` icon.
- Dialog pattern: no new file added. `components/ui/dialog.tsx` (added in `01-design-system`, unmodified) already exposes `DialogTitle`, `DialogDescription`, and `DialogFooter` and already renders through the Ghost AI theme tokens (`--popover`, `--popover-foreground`, `--muted`, etc., aliased in `globals.css`). Verified this satisfies the spec's title/description/footer-actions requirement without building a concrete dialog instance, per the spec's "do not build actual dialogs yet."
- Verified: both new components are client components (`"use client"`, needed for the toggle/close interactivity). Temporarily mounted both in a throwaway route with local `useState` to drive `isSidebarOpen`/`isOpen`; confirmed the toggle button swaps icons, the sidebar slides in/out without shifting `main` content, and both tabs render. Ran `npx tsc --noEmit`, `npx eslint components/editor`, and `npx next build` — all pass with no errors. The throwaway route was removed after verification.

### `01-design-system`

- Installed and initialized `shadcn/ui` (`components.json`, style `radix-nova`, base color `neutral`, CSS variables enabled).
- Added shadcn components: Button, Card, Dialog, Input, Tabs, Textarea, ScrollArea (`components/ui/*`, unmodified from generated output).
- Installed `lucide-react` (pulled in automatically as the configured icon library).
- Created `lib/utils.ts` re-exporting `cn()` from the official `cn` package (shadcn's current default — a compiled drop-in replacement for `clsx` + `tailwind-merge`).
- Defined the full Ghost AI dark theme as CSS custom properties in `app/globals.css` (`--bg-base`, `--bg-surface`, `--bg-elevated`, `--bg-subtle`, `--border-default`, `--border-subtle`, `--text-primary/secondary/muted/faint`, `--accent-primary`, `--accent-primary-dim`, `--accent-ai`, `--accent-ai-text`, `--state-error/success/warning`), matching `context/ui-context.md`.
- Mapped those tokens into Tailwind utilities via `@theme inline` (`bg-base`, `bg-surface`, `bg-elevated`, `bg-subtle`, `border-surface-border`, `border-subtle-border`, `text-copy-primary/secondary/muted/faint`, `text-brand`/`bg-brand`, `bg-accent-dim`, `text-ai`/`bg-ai`, `text-ai-text`, `error`/`success`/`warning`).
- Mapped shadcn's semantic tokens (`background`, `foreground`, `card`, `popover`, `primary`, `secondary`, `muted`, `accent`, `destructive`, `border`, `input`, `ring`, `sidebar-*`, `chart-*`) onto the Ghost AI palette so every shadcn primitive is dark-themed by default.
- Fixed `--font-sans`/`--font-heading` to resolve to `--font-geist-sans` (was circular in the generated file) and added `--font-mono` → `--font-geist-mono`.
- Added the `dark` class to `<html>` in `app/layout.tsx` so shadcn's `dark:` variant refinements apply (app is dark-only, no theme toggle).
- Verified: `npx tsc --noEmit`, `npx eslint .`, and `npx next build` all pass; a temporary route importing all 7 components (Button, Card, Dialog, Input, Tabs, Textarea, ScrollArea) built and served successfully, and the compiled CSS was inspected to confirm all theme variables resolve to the dark hex values (no light/oklch defaults left). The temporary route was removed after verification.

## In Progress

- None. `02-editor` (navbar + project sidebar shell) is complete.

## Next Up

- No page currently mounts `EditorNavbar`/`ProjectSidebar` or owns the shared `isSidebarOpen` state — that wiring belongs to whichever feature unit builds the actual editor route/canvas.
- Select the next feature unit (see `context/feature-specs/`) — none beyond `02-editor` exists yet.

## Open Questions

- None.

## Architecture Decisions

- `EditorNavbar` and `ProjectSidebar` are controlled components (props only, no internal open/closed state) so a single parent (the future editor page) owns sidebar state and both components stay in sync. Neither component pushes/reflows page content — the sidebar is `fixed`/floating per `context/ui-context.md`'s layout pattern.
- Design tokens live in `app/globals.css` as CSS custom properties, mapped into Tailwind via `@theme inline`, per `context/ui-context.md`. shadcn's own semantic tokens (`--primary`, `--card`, etc.) are aliased onto these instead of using shadcn's default palette, so all `components/ui/*` primitives automatically match the Ghost AI theme without modification.
- The spec named `libs/utils.ts`; the actual path used is `lib/utils.ts`, consistent with `context/architecture-context.md`'s `lib/` boundary and the current shadcn CLI's default alias (`@/lib/utils`). Treated as a naming typo in the spec, not a scope change.
- App is dark-only: the `dark` class is applied unconditionally on `<html>` rather than implementing a light/dark toggle.

## Session Notes

- shadcn CLI in this environment is v4.21.0, using presets (`-p nova`) and a `radix` base rather than the older `init` flow — resolved by reading its `--help` output rather than assuming older CLI conventions, per the Next.js/tooling drift called out in `AGENTS.md`.
- `lucide-react` in this environment is v1.x (ESM-only), not the legacy 0.x line — icon module resolution was spot-checked to confirm imports work.
