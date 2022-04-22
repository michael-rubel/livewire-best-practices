![Livewire Best Practices](https://avatars.githubusercontent.com/u/51960834?s=100&v=4)

## Livewire Best Practices
[![All Contributors](https://img.shields.io/badge/all_contributors-1-orange.svg)](#contributors-)
[![StandWithUkraine](https://raw.githubusercontent.com/vshymanskyy/StandWithUkraine/main/badges/StandWithUkraine.svg)](https://github.com/vshymanskyy/StandWithUkraine/blob/main/docs/README.md)

This repository is a curated list of general advisories on how to use [Livewire framework](https://github.com/livewire/livewire) to meet enterprise concerns regarding security, performance, and maintenance of Livewire components.

### Short introduction
My name is [Michael Rubel](https://github.com/michael-rubel) and I started using the Livewire framework in 2019 when it was new and barely stable. Back in the day, I was impressed with how fast dynamic UIs can be shipped without even using JavaScript. But like any software solution, it had its pitfalls, and I had to deal with them. The main goal of this repository is to collect the most important experiences that you need to consider when working with Livewire.

## Contents
* [Coming soon...](#coming-soon)

### ✨ The Golden rule of performant Livewire
```html
Don't use big objects in Livewire components!
```

Better if you can omit using objects at all if possible, and use only primitive types: strings, integers, arrays, etc. That's because Livewire serializes/deserializes your component's payload each request to the server to share the state between frontend & backend.

What is considered a big object?
- Any instance as huge as *Eloquent Model* is already big enough for the Livewire component. For example, if you have the component that represents the user profile (email and username), it's better to pass these parameters to properties as strings instead of assigning the whole model and then extracting attributes in the view.

🔥 Exception here is using of Route Model Binding, i.e. you pass only an ID or UUID of the model, and then map the model attributes to the component's properties. Remember: don't assign a whole model, but its attributes only. To prevent manually mapping model attributes, you can use [Loop Functions](https://github.com/michael-rubel/laravel-loop-functions#assign-eloquent-model-attributes-to-class-properties) package.

Note: if you use [full-page components](https://laravel-livewire.com/docs/2.x/rendering-components#page-components), it's recommended to fetch objects in the full-page component itself, and then pass them downstairs to the nested ones as primitive types.

### 💡 Use *debounce*, *lazy* & *defer* wire:model's modifiers
To avoid unnecessary requests to the server, you can use wire:model's modifiers based on requirements for a particular input.
- `debounce` - waits for a particular amount of time after the keystroke on the input before sending a server request.
- `lazy` - server request will be fired only when the user clicks away from the field.
- `defer` - saves the new value internally and passes it to the next request that may come from other input fields or other button clicks

### 🕵️ Don't pass sensitive data to the components
Prevent situations that may lead to passing sensitive data to the Livewire components, because they can be easily accessed from the client side. Always filter your models using `$hidden` property or explicitly filter the data you are fetching.

### 📦 Use computed properties to access database
You can use [computed properties](https://laravel-livewire.com/docs/2.x/properties#computed-properties) to avoid unnecessary database queries. The properties are cached within the component's lifecycle and do not perform additional SQL queries on subsequent requests when updating the state of an already mounted component.

### 👨‍💻 Use artisan commands to create, move and rename components
Livewire has [ready-to-use](https://laravel-livewire.com/docs/2.x/reference#artisan-commands) artisan commands to create, move, rename components, etc.
For example, instead of manually renaming, which could be error-prone, you can use the following command:
- `php artisan livewire:move Path/To/Component New/Path/To/Component`

### 🧵 Keep component nesting level at 1
If you had a Livewire component (0) that includes another Livewire component (1), then you shouldn't nest it deeper (2+). Too much nesting can create a headache when dealing with DOM diffing issues.

### 🌎 Use Form Request rules for validation
Livewire doesn't support [Form Requests](https://laravel.com/docs/9.x/validation#form-request-validation) internally, but instead of hardcoding array of validation rules in the component, you may get it from Form Request:
```php
public function rules(): array
{
    return (new MyFormRequest)->rules();
}
```

This way you can reuse the same validation rules in different application layers, for example in API endpoints.

### ☔ Use event listeners instead of polling if possible
Instead of constantly polling to refresh your data, you may use [event listeners](https://laravel-livewire.com/docs/2.x/events#event-listeners) to perform the component update only on specific actions from another component.

## Contribution
If you see any things we can improve about this list, or you want to add some tips yourself, contributions are greatly welcomed. :)
