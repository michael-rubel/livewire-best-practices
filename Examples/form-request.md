### 🌎 Use Form Request rules for validation

:x: Bad:
```php
public function rules(): array
{
    return [
        'field1' => ['required', 'string'],
        'field2' => ['required', 'integer'],
        'field3' => ['required', 'boolean'],
    ];
}
```

:heavy_check_mark: Good:
```php
public function rules(): array
{
    return (new MyFormRequest)->rules();
}
```
