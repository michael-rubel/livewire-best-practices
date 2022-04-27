### 🗺️ Use Route Model Binding to fetch the model

Example `mount`:

```php
public function mount(User $user): void
{
    $this->fill($user);
}
```

Bad:
```html
<livewire:profile :user="auth()->user()" /> 
```

Good:
```html
<livewire:profile :user="auth()->user()->uuid" /> 
```
