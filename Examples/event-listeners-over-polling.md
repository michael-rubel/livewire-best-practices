### ☔ Prefer to use event listeners over polling

- Bad:
```html
<div wire:poll>
    User Content
</div>
```

- Good:

*In component to update:*
```php
protected $listeners = [
    'userUpdated' => '$refresh',
];
```

*In component to send signal from:*
```php
$this->emit('userUpdated');
```
