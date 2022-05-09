### 💡 Use *debounce*, *lazy* & *defer* wire:model's modifiers

Bad:
```html
<input wire:model="email">
```

:heavy_check_mark: Good:
```html
<input wire:model.debounce.500ms="email">
```