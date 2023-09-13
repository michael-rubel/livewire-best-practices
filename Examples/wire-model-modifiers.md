### 💡 Avoid using *live* wire:model modifier where possible

:x: Bad:
```html
<input wire:model.live="email">
```

:heavy_check_mark: Better:
```html
<input wire:model.live.debounce.500ms="email">
```

:heavy_check_mark: Even better:
```html
<input wire:model.blur="email">
```

:heavy_check_mark: Ideal:
```html
<input wire:model="email">
```
