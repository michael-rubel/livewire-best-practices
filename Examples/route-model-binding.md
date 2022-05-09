### 🗺️ Use Route Model Binding to fetch the model

Example `mount`:

```php
public function mount(User $user): void
{
    $this->fill($user);
}
```

:x: Bad:
```html
<livewire:profile :user="auth()->user()" /> 
```

:heavy_check_mark: Good:
```html
<livewire:profile :user="auth()->user()->uuid" /> 
```
