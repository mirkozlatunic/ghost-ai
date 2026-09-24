We need the base Chrome components that frame every editor screen - the top nav bar and the left sidebar shell. These will be reused and extended in every chapter that follows.

### Editor Navbar

Create `components/editor/editor-navbar.tsx`.

Requirements:

- Fixed high top nav bar
- Left, center, and right sections
- Left section contains the sidebar toggle button
- `PanelLeftOpen` / `PanelLeftClose` icons based on sidebar state
- Right section says empty for now
- dark background and a subtle bottom border

### Project Sidebar

Create `components/editor/project-sidebar.tsx`

Requirements:

- Sidebar should float above the editor canvas
- Opening it should not push page content
- Slides in from the left
- accept `isOpen` prop
- header with `Project` title + close button
- shdacn `Tabs`"
  - My Project
  - Shared
- both tabs show empty placeholder state
- full-width `New Project` button at the bottom with `Plus` icon

### Dialog Pattern

Use the existing color tokens from `global.css` for dialog styling.

Support:

- title
- description
- footer actions

Do not build actual dialogs yet.

### Check when done

- new component compile withouth TypeScript errors
- no lint errors
- dialog pattern is ready for future use
