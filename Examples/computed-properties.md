### 📦 Use computed properties to access database

Bad:
```php
public function render(): View
{
    $countries = Country::select('name', 'code')
        ->orderBy('name')
        ->get();

    return view('livewire.select-country', ['countries' => $countries]);
}
```

Good:
```php
public function getCountriesProperty(): Collection
{
    return Country::select('name', 'code')
        ->orderBy('name')
        ->get();
}
```
