# Blade Components Guide

Extracting repeated UI elements into Blade Components (`resources/views/components/`) makes your codebase cleaner, smaller, and easier to maintain.

## 1. Anonymous Blade Components

For most UI elements (buttons, inputs, cards), Anonymous Blade Components are the best choice. They don't require a PHP class, just a `.blade.php` file in the `components` directory.

### Example: Card Component (`resources/views/components/card.blade.php`)

Raw HTML typically looks like this, repeated many times:
```html
<div class="bg-surface-container-lowest p-6 rounded-xl border border-outline-variant form-shadow">
    <!-- Content -->
</div>
```

**Component Definition:**
```blade
<div {{ $attributes->merge(['class' => 'bg-surface-container-lowest p-6 rounded-xl border border-outline-variant shadow-sm']) }}>
    {{ $slot }}
</div>
```

**Usage in View:**
```blade
<x-card class="mb-4">
    <h2 class="text-xl">Card Title</h2>
    <p>Card content goes here.</p>
</x-card>
```

## 2. Using `@props` for Dynamic Classes

When an element has variations (like button colors), use `@props` to define defaults and handle the logic.

### Example: Button Component (`resources/views/components/button.blade.php`)

```blade
@props([
    'variant' => 'primary', // primary, secondary, outline
    'type' => 'button',
    'icon' => null
])

@php
    $baseClasses = 'flex items-center justify-center gap-2 px-6 py-3 rounded-xl font-label-md font-bold transition-all active:scale-[0.98]';
    
    $variantClasses = match($variant) {
        'primary' => 'bg-primary text-on-primary hover:bg-primary-container shadow-md',
        'secondary' => 'bg-secondary text-on-secondary hover:bg-secondary-container',
        'outline' => 'border border-outline-variant text-on-surface hover:bg-surface-container',
        default => 'bg-surface-container text-on-surface',
    };
@endphp

<button type="{{ $type }}" {{ $attributes->merge(['class' => "$baseClasses $variantClasses"]) }}>
    @if($icon)
        <span class="material-symbols-outlined text-[18px]">{{ $icon }}</span>
    @endif
    
    {{ $slot }}
</button>
```

**Usage:**
```blade
<x-button variant="primary" type="submit" icon="save">
    Save Changes
</x-button>

<x-button variant="outline" href="{{ route('cancel') }}">
    Cancel
</x-button>
```

## 3. Form Input Components

Form inputs often have repeated structures (label, input, error message).

### Example: Input Group (`resources/views/components/input-group.blade.php`)

```blade
@props([
    'name',
    'label',
    'type' => 'text',
    'placeholder' => '',
    'required' => false,
    'value' => null
])

<div class="flex flex-col gap-2">
    <label class="font-label-md text-on-surface ml-1" for="{{ $name }}">
        {{ $label }}
        @if($required) <span class="text-error">*</span> @endif
    </label>
    
    <div class="relative">
        <input 
            type="{{ $type }}" 
            id="{{ $name }}" 
            name="{{ $name }}" 
            value="{{ old($name, $value) }}" 
            placeholder="{{ $placeholder }}" 
            {{ $required ? 'required' : '' }}
            {{ $attributes->merge(['class' => 'w-full px-4 py-3 rounded-xl border border-outline-variant bg-surface focus:border-primary focus:ring-1 focus:ring-primary transition-all outline-none']) }}
        />
    </div>
    
    @error($name)
        <p class="text-error text-[12px] ml-1">{{ $message }}</p>
    @enderror
</div>
```

**Usage:**
```blade
<x-input-group 
    name="email" 
    label="Email Address" 
    type="email" 
    placeholder="john@example.com" 
    required="true" 
/>
```

## 4. Icons

Instead of writing out the full Material Symbols span every time, create an icon component.

### Example: Icon (`resources/views/components/icon.blade.php`)

```blade
@props(['name', 'weight' => '400', 'fill' => '0'])

<span 
    {{ $attributes->merge(['class' => 'material-symbols-outlined']) }} 
    style="font-variation-settings: 'FILL' {{ $fill }}, 'wght' {{ $weight }};"
>
    {{ $name }}
</span>
```

**Usage:**
```blade
<x-icon name="dashboard" class="text-primary text-[24px]" />
```
