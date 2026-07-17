---
name: stitch-laravel-converter
description: Analyzes and converts raw HTML/Tailwind from design exports (Stitch/Figma) into clean Laravel Blade components, layouts, and views following DRY (Don't Repeat Yourself) principles. Use this when the user asks to functionalize or clean up raw HTML designs.
license: MIT
metadata:
  version: "1.0.0"
  domain: frontend
  triggers: stitch, convert design, clean html, create layout, blade components, dry, tailwind html to blade
  role: frontend engineer
  scope: implementation
  output-format: code
---

# Stitch to Laravel Blade Converter

Specialist in transforming raw HTML designs (usually with long and repetitive Tailwind CDNs) into a clean, modular, and efficient Laravel view architecture.

## Core Workflow

1. **Analyze Global Structure** — Inspect the raw files to find elements that are repeated across pages (such as `<head>`, CDN links, Navbar/Header, Sidebar, Footer, and `<script>` blocks).
2. **Create Master Layout** — Create a main layout file (e.g., `resources/views/layouts/app.blade.php` or `<x-app-layout>`). Move all global elements into this file and use `@yield('content')` or `{{ $slot }}` for the page-specific content.
3. **Extract Reusable Components** — Identify frequently used small UI elements (such as buttons, input forms, cards, alerts) and separate them into the `resources/views/components/` directory so they can be called using `<x-button>`, `<x-input>`, etc.
4. **Clean Up Specific Pages (Views)** — Remove the global structure (`<html>`, `<head>`, `<body>`, Navbar, etc.) from specific pages (like `login.blade.php`, `dashboard.blade.php`).
5. **Implement Layout & Components** — Wrap the specific page content with `@extends('layouts.app')` and `@section('content')` (or `<x-app-layout>`). Replace the raw HTML with Blade component calls.
6. **Organize Tailwind & Vite** — Migrate the Tailwind CDN configuration into the proper `tailwind.config.js` and set up `resources/css/app.css` using Vite. Remove the CDN scripts.
7. **Add Interactivity** — Use Alpine.js (`x-data`, `x-show`, `@click`) to replace static CSS states for elements like dropdowns, modals, and tabs.

## Reference Guide

Load detailed guidance based on context:

| Topic | Reference | Load When |
|-------|-----------|-----------|
| Layout & Structure | `references/layout-structure.md` | Setting up master layouts, extracting navbars, headers, sidebars, and footers |
| Blade Components | `references/blade-components.md` | Extracting UI elements (buttons, inputs, cards) into reusable `<x-name>` components |
| Interactivity (Alpine.js) | `references/alpinejs-integration.md` | Making static dropdowns, modals, and tabs clickable/interactive |
| Asset Pipeline (Vite) | `references/vite-migration.md` | Migrating from Tailwind CDN to a production-ready Vite build |

## Constraints

### MUST DO
- Apply **DRY** (Don't Repeat Yourself) principles. There should not be the same `<!DOCTYPE html>`, `<head>`, or Tailwind CDN link declaration in more than one file.
- Maximize the use of Blade directives (`@extends`, `@section`, `@include`, `@push`, `@stack`, or slot components).
- Maintain HTML semantics and *accessibility*.
- Use `@push('scripts')` or `<x-slot:scripts>` for page-specific *javascript*.
- Convert static CSS display toggles into Alpine.js states.
- Migrate styling from CDN to Vite (`@vite(['resources/css/app.css', 'resources/js/app.js'])`).

### MUST NOT DO
- Leave `<html>`, `<head>`, or `<body>` tags inside specific view files (except for the master layout).
- Repeat the design color configuration (Tailwind config JS) in every view.
- Create a single `.blade.php` file containing thousands of HTML lines without any component extraction.

## Code Templates

Use these as a starting point for conversion.

### Master Layout (`resources/views/layouts/app.blade.php`)

```blade
<!DOCTYPE html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}" class="light">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>@yield('title', config('app.name'))</title>
    
    <!-- Global Scripts / Styles (Tailwind CDN / Vite) -->
    <script src="https://cdn.tailwindcss.com?plugins=forms,container-queries"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet"/>
    <link href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:wght,FILL@100..700,0..1&display=swap" rel="stylesheet"/>
    
    <!-- Specific Configuration -->
    <script id="tailwind-config">
        tailwind.config = {
            // Tailwind configuration...
        }
    </script>
    
    @stack('styles')
</head>
<body class="bg-background text-on-surface font-body-md overflow-hidden">
    <!-- Global Navbar / Sidebar -->
    @include('layouts.partials.navbar')
    @include('layouts.partials.sidebar')

    <!-- Page Specific Content -->
    <main>
        @yield('content')
    </main>

    <!-- Global Footer -->
    @include('layouts.partials.footer')

    <!-- Page Specific Scripts -->
    @stack('scripts')
</body>
</html>
```

### Specific View (`resources/views/pages/dashboard.blade.php`)

```blade
@extends('layouts.app')

@section('title', 'Dashboard')

@section('content')
    <div class="max-w-[1280px] mx-auto p-8">
        <h1 class="font-headline-lg text-headline-lg">Welcome</h1>
        
        <!-- Blade Component Call -->
        <x-card>
            <x-button color="primary">Save</x-button>
        </x-card>
    </div>
@endsection

@push('scripts')
<script>
    // Specific JS for the dashboard
</script>
@endpush
```

### Simple Blade Component (`resources/views/components/button.blade.php`)

```blade
@props([
    'color' => 'primary', // default
    'type' => 'button'
])

@php
    $colorClasses = match($color) {
        'primary' => 'bg-primary text-white hover:opacity-90',
        'secondary' => 'bg-secondary text-white hover:opacity-90',
        'outline' => 'border border-outline text-on-surface hover:bg-surface-container',
        default => 'bg-surface-container text-on-surface',
    };
@endphp

<button type="{{ $type }}" {{ $attributes->merge(['class' => "px-6 py-2.5 rounded-full font-label-md transition-all $colorClasses"]) }}>
    {{ $slot }}
</button>
```

## Validation Checkpoints

| Stage | Expected Result |
|-------|-----------------|
| Initial Analysis | Finding all duplicate elements across raw HTML pages. |
| Layout Creation | The master layout has the `<html>`, `<head>`, and global script declarations. |
| View Integration | Page files (like `dashboard.blade.php`) only contain their semantic inner tags without repeating the `<body>` tag. |
| Rendering Test | The pages look exactly like the original design but with clean file and folder structures. |
