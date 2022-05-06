### 🌎 Use Form Request rules for validation

Bad:
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

Good:
```php
public function rules(): array
{
    return (new MyFormRequest)->rules();
}
```
