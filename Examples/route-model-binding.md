### 🗺️ Use Route Model Binding to fetch the model

Example component:

```php
class Profile extends Component
{
    /**
     * @var string
     */
    public string $email;

    /**
     * @var string|null
     */
    public ?string $username = null;

    /**
     * @param User $user
     *
     * @return void
     */
    public function mount(User $user): void
    {
        $this->email    = $user->email;
        $this->username = $user->username;
    }
}
```

Bad:
```html
<livewire:profile :user="$user" /> 
```

Good:
```html
<livewire:profile :user="$user->uuid" /> 
```
