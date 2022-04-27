### 🗺️ Use Route Model Binding to fetch the model

Example `mount`:

```php
public function mount(User $user): void
{
    $this->email    = $user->email;
    $this->username = $user->username;
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
