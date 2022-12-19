### 💡 Use *lazy*, *defer* & *debounce* wire:model's modifiers

:x: Bad:
```html
<input wire:model="email">
```

:heavy_check_mark: Good:
```html
<input wire:model.lazy="email">
```
