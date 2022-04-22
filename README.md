![Livewire Best Practices](https://avatars.githubusercontent.com/u/51960834?s=100&v=4)

## Livewire Best Practices
[![StandWithUkraine](https://raw.githubusercontent.com/vshymanskyy/StandWithUkraine/main/badges/StandWithUkraine.svg)](https://github.com/vshymanskyy/StandWithUkraine/blob/main/docs/README.md)

This repository is a curated list of general recommendations on how to use [Laravel Livewire framework](https://github.com/livewire/livewire) to meet enterprise concerns regarding security, performance, and maintenance of Livewire components.

### Short introduction
My name is [Michael Rubel](https://github.com/michael-rubel) and I started using the Livewire framework in 2019 when it was new and barely stable. Back in the day, I was impressed with how fast dynamic UIs can be shipped without even using JavaScript. But like any software solution, it had its pitfalls, and I had to deal with them. The main goal of this repository is to collect the most important experiences that you need to consider when working with Livewire.

---
### ✨ The Golden rule of performant Livewire
```html
Don't pass large objects to Livewire components!
```

Avoid passing objects to component's `mount` method or properties if possible. Use primitive types: strings, integers, arrays, etc. That's because Livewire serializes/deserializes your component's payload each request to the server to share the state between frontend & backend. If you need to work on objects, you can create them inside a method or computed property, and then return the result of processing as an array or paginated collection if needed.

What is considered a large object?
- Any instance as huge as *Eloquent Model* is already big enough for the Livewire component to slow down the component lifecycle. For example, if you have the component that represents the user profile (email and username), it's better to pass these parameters to properties as strings instead of assigning the whole model and then extracting attributes in the view.

Note: if you use [full-page components](https://laravel-livewire.com/docs/2.x/rendering-components#page-components), it's recommended to fetch objects in the full-page component itself, and then pass them downstairs to the nested ones as primitive types.

---
### 🗺️ Use Route Model Binding to fetch the model
Pass only an ID or UUID to the `mount` method, then map the model attributes to component properties. Remember: don't assign a whole model, but its attributes only. To prevent manually mapping model attributes, you can use [Loop Functions](https://github.com/michael-rubel/laravel-loop-functions#assign-eloquent-model-attributes-to-class-properties) package.

---
### 💡 Use *debounce*, *lazy* & *defer* wire:model's modifiers
To avoid unnecessary requests to the server, you can use wire:model's modifiers based on requirements for a particular input.
- `debounce` - waits for a particular amount of time after the keystroke on the input field before sending a server request.
- `lazy` - the request will be sent only when the user clicks away from the input field.
- `defer` - saves the new value internally and passes it to the next request that may come from other input fields or other button clicks.

---
### 🕵️ Don't pass sensitive data to the components
Prevent situations that may lead to passing sensitive data to the Livewire components, because they can be easily accessed from the client side. Always hide sensitive attributes of your models using `$hidden` property or explicitly filter the data you are fetching.

---
### 📦 Use computed properties to access database
You can use [computed properties](https://laravel-livewire.com/docs/2.x/properties#computed-properties) to avoid unnecessary database queries. Computed properties are cached within the component's lifecycle and do not perform additional SQL queries on subsequent requests when updating the state of an already mounted component.

---
### 👨‍💻 Use artisan commands to create, move and rename components
Livewire has [built-in artisan commands](https://laravel-livewire.com/docs/2.x/reference#artisan-commands) to create, move, rename components, etc.
For example, instead of manually renaming files, which could be error-prone, you can use the following command:
- `php artisan livewire:move Old/Path/To/Component New/Path/To/Component`

---
### 🧵 Keep component nesting level at 1
If you had a Livewire component (0) that includes another Livewire component (1), then you shouldn't nest it deeper (2+). Too much nesting can make a headache when dealing with DOM diffing issues.

---
### 🌎 Use Form Request rules for validation
Livewire doesn't support [Form Requests](https://laravel.com/docs/9.x/validation#form-request-validation) internally, but instead of hardcoding array of validation rules in the component, you may get it directly from Form Request:
```php
public function rules(): array
{
    return (new MyFormRequest)->rules();
}
```

This way you can reuse the same validation rules in different application layers, for example in API endpoints.

---
### ☔ Use event listeners instead of polling if possible
Instead of constantly [polling](https://laravel-livewire.com/docs/2.x/polling#polling-background) the page to refresh your data, you may use [event listeners](https://laravel-livewire.com/docs/2.x/events#event-listeners) to perform the component update only after specific task, initiated from another component.

---
## Contributors
[![GitHub Contributors Image](https://contrib.rocks/image?repo=michael-rubel/livewire-best-practices)](https://github.com/michael-rubel/livewire-best-practices/graphs/contributors)
