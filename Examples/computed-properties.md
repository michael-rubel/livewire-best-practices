### 📦 Use computed properties to access database

:x: Bad:
```php
public function render(): View
{
    $countries = Country::select('name', 'code')
        ->orderBy('name')
        ->get();

    return view('livewire.select-country', ['countries' => $countries]);
}
```

:heavy_check_mark: Good:
```php
public function getCountriesProperty(): Collection
{
    return Country::select('name', 'code')
        ->orderBy('name')
        ->get();
}
```
