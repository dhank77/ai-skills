# Alpine.js Integration Guide

Static HTML designs often rely on CSS hover states or simply show all elements expanded (like open modals or dropdowns) to demonstrate the UI. To make these functional without writing complex vanilla JS, use Alpine.js.

## 1. Setup

Ensure Alpine.js is included in your `layouts.app` or `resources/js/app.js` (via Vite).

## 2. Dropdowns & Menus

Static designs often display the dropdown menu openly. We need to hide it by default and toggle it on click.

**Static HTML:**
```html
<div class="relative">
    <button>Profile</button>
    <div class="absolute right-0 mt-2 bg-white shadow-lg">
        <a href="#">Settings</a>
        <a href="#">Logout</a>
    </div>
</div>
```

**Alpine.js Version:**
```blade
<div class="relative" x-data="{ open: false }" @click.away="open = false">
    <button @click="open = !open">Profile</button>
    
    <div 
        x-show="open" 
        x-transition
        class="absolute right-0 mt-2 bg-white shadow-lg"
        style="display: none;"
    >
        <a href="#">Settings</a>
        <a href="#">Logout</a>
    </div>
</div>
```
*(Note: Always add `style="display: none;"` or `x-cloak` to prevent UI flickering before Alpine initializes).*

## 3. Modals

Modals in static designs are usually placed at the bottom of the HTML and use inline styles or Tailwind classes like `hidden` or `flex` to show/hide them.

**Static HTML:**
```html
<!-- Button somewhere -->
<button onclick="document.getElementById('modal').style.display='flex'">Open Modal</button>

<!-- Modal -->
<div id="modal" class="fixed inset-0 bg-black/50 hidden items-center justify-center">
    <div class="bg-white p-6">
        <h2>Modal Title</h2>
        <button onclick="document.getElementById('modal').style.display='none'">Close</button>
    </div>
</div>
```

**Alpine.js Version:**
Instead of scattering `onclick` events, bind the modal to Alpine state. Often, this state is best placed at the parent component or managed via Alpine `$dispatch`.

```blade
<div x-data="{ showModal: false }">
    <!-- Trigger -->
    <button @click="showModal = true" class="...">Open Modal</button>

    <!-- Modal -->
    <div 
        x-show="showModal" 
        class="fixed inset-0 bg-black/50 z-50 flex items-center justify-center"
        style="display: none;"
    >
        <!-- Backdrop click to close -->
        <div class="absolute inset-0" @click="showModal = false"></div>
        
        <!-- Modal Content -->
        <div class="relative bg-white p-6 z-10" @click.stop>
            <h2>Modal Title</h2>
            <button @click="showModal = false">Close</button>
        </div>
    </div>
</div>
```

## 4. Tabs

Tabs are another common UI element that needs to be functionalized.

**Alpine.js Version:**
```blade
<div x-data="{ activeTab: 'tab1' }">
    <!-- Tab Headers -->
    <div class="flex border-b">
        <button 
            @click="activeTab = 'tab1'" 
            :class="activeTab === 'tab1' ? 'border-b-2 border-primary text-primary' : 'text-gray-500'"
            class="px-4 py-2"
        >Tab 1</button>
        <button 
            @click="activeTab = 'tab2'" 
            :class="activeTab === 'tab2' ? 'border-b-2 border-primary text-primary' : 'text-gray-500'"
            class="px-4 py-2"
        >Tab 2</button>
    </div>

    <!-- Tab Contents -->
    <div class="p-4">
        <div x-show="activeTab === 'tab1'">Content for Tab 1</div>
        <div x-show="activeTab === 'tab2'" style="display: none;">Content for Tab 2</div>
    </div>
</div>
```
