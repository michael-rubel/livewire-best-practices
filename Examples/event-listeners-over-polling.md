### ☔ Prefer to use event listeners over polling

:x: Bad:
```html
<div wire:poll>
    User Content
</div>
```

:heavy_check_mark: Good:

*Define the listener in the component using `On` attribute:*
```php
class Dashboard extends Component
{
    #[On('post-created')] 
    public function updatePostList($title)
    {
        // ...
    }
}
```

*Dispatch the event in every other component:*
```php
$this->dispatch('post-created'); 
```
