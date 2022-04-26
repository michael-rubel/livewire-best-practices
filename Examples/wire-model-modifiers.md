### 💡 Use *debounce*, *lazy* & *defer* wire:model's modifiers

Bad:
```html
<input wire:model="email">
```

Good:
```html
<input wire:model.debounce.500ms="email">
```