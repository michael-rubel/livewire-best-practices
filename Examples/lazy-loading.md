📈 Use lazy loading

Example:

Add the `placeholder` method in the component class:
```php
public function placeholder(): string
{
    return <<<'HTML'
        <div>
            <!-- Loading spinner... -->
            <svg>...</svg>
        </div>
    HTML;
}
```

Add the component with the `lazy` attribute:
```html
<livewire:component-name lazy />
```