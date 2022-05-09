### 🔗 Entangle your live data

Instead of using component like this (server-bound):

```html
<div>
    <input wire:model="count" type="number">
    <button wire:click="increment">+</button>
</div>
```

You can do it like this:

```html
<div x-data="{ count: @entangle('count') }">
    <input x-model="count" type="number">
    <button @click="count++">+</button>
</div>
```
