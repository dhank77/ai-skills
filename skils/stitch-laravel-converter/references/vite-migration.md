# Vite Asset Migration Guide

Static UI exports often use Tailwind CSS via a CDN (`<script src="https://cdn.tailwindcss.com"></script>`). This is unacceptable for production Laravel applications. We must migrate the styling to Laravel's Vite build system.

If the project uses **Tailwind CSS v4**, use CSS-first configuration instead of generating `tailwind.config.js`.

## Tailwind CSS v4 Path

### 1. Extract Theme Tokens From HTML

Find the `<script id="tailwind-config">` block in the raw HTML.

**Raw HTML:**
```html
<script id="tailwind-config">
    tailwind.config = {
        theme: {
            extend: {
                colors: {
                    primary: "#00288e"
                }
            }
        }
    }
</script>
```

**Action:**
Translate `theme.extend` tokens into CSS `@theme` declarations in `resources/css/app.css`.

**`resources/css/app.css`:**
```css
@import "tailwindcss";

@theme {
    --color-primary: #00288e;
}

/* custom styles */
.custom-scrollbar::-webkit-scrollbar {
    width: 6px;
}
```

If the project uses a different Tailwind major version, keep the JS config path instead.

## 2. Extracting Custom CSS

Find any `<style>` blocks in the raw HTML. These usually contain custom scrollbars, font imports, or specific overrides.

**Raw HTML:**
```html
<style>
    .custom-scrollbar::-webkit-scrollbar { width: 6px; }
</style>
```

**Action:**
Move these styles into `resources/css/app.css`.

**`resources/css/app.css`:**
```css
@import "tailwindcss";

@theme {
    --color-primary: #00288e;
}

@layer utilities {
    .custom-scrollbar::-webkit-scrollbar {
        width: 6px;
    }
}
```

## 3. Updating the Master Layout

Remove the CDN script and custom `<style>`/`<script>` blocks from your `layouts.app.blade.php`, and replace them with the `@vite` directive.

**Before:**
```blade
<head>
    <!-- ... -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script id="tailwind-config"> ... </script>
    <style> ... </style>
</head>
```

**After:**
```blade
<head>
    <!-- ... -->
    
    <!-- Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:wght,FILL@100..700,0..1&display=swap" rel="stylesheet">

    <!-- Vite Assets -->
    @vite(['resources/css/app.css', 'resources/js/app.js'])
</head>
```
