### 📦 Use computed properties to access database

:x: Bad:
```php
public function countries(): Collection
{
    return Country::select('name', 'code')
        ->orderBy('name')
        ->get();
}
```

:heavy_check_mark: Good:
```php
#[Computed]
public function countries(): Collection
{
    return Country::select('name', 'code')
        ->orderBy('name')
        ->get();
}
```
