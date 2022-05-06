### 🌎 Use Form Request rules for validation

Bad:
```php
public function rules(): array
{
    return [
        'field1' => ['required', "unique:{$model->getTable()},column_name,{$model->id}"],
        'field2' => ['required', 'integer'],
        'field3' => ['required', 'boolean'],
    ];
}
```

Good:
```php
// FormRequest
public function rules(): array
{
    return static::baseRules($this->model);
}

public static function baseRules(Model $model): array
{
    return [
        'field1' => ['required', "unique:{$model->getTable()},column_name,{$model->id}"],
        'field2' => ['required', 'integer'],
        'field3' => ['required', 'boolean'],
    ];
}

// LivewireComponent
protected function rules(): array
{
    return FormRequest::baseRules($this->model);
}
```
