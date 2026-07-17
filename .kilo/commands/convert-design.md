---
description: Convert raw HTML/Tailwind design exports into clean Laravel Blade layouts, components, and Vite-ready assets.
agent: code
subtask: false
---
Use `stitch-laravel-converter` behavior for this command.
Analyze repeated structure such as `<head>`, navbar, sidebar, footer, and script blocks.
Create or update a master layout in `resources/views/layouts/`.
Extract repeated UI pieces into reusable Blade components.
Move page-specific content into view files that extend the layout.
Migrate Tailwind CDN setup into `tailwind.config.js` and `resources/css/app.css`.
Replace static CSS state toggles with Alpine.js where appropriate.

## References

- `skils/stitch-laravel-converter/references/layout-structure.md`
- `skils/stitch-laravel-converter/references/blade-components.md`
- `skils/stitch-laravel-converter/references/alpinejs-integration.md`
- `skils/stitch-laravel-converter/references/vite-migration.md`
