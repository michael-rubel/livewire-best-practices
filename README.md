![Livewire Best Practices](https://avatars.githubusercontent.com/u/51960834?s=100&v=4)

## Livewire Best Practices
[![StandWithUkraine](https://raw.githubusercontent.com/vshymanskyy/StandWithUkraine/main/badges/StandWithUkraine.svg)](https://github.com/vshymanskyy/StandWithUkraine/blob/main/docs/README.md)

This repository is a curated list of general recommendations on how to use [Laravel Livewire framework](https://github.com/livewire/livewire) to meet enterprise concerns regarding security, performance, and maintenance of Livewire components.

### Short introduction
My name is [Michael Rubél](https://github.com/michael-rubel) and I started using the Livewire framework in 2019 when it was new and barely stable. Back in the day, I was impressed with how fast dynamic UIs can be shipped without even using JavaScript. But like any software solution, it had its pitfalls, and I had to deal with them. The main goal of this repository is to collect the most important experiences that you need to consider when working with Livewire.

Let's begin...

---
### 🌳 Always set up root element
Livewire requires a [root element](https://laravel-livewire.com/docs/2.x/troubleshooting#root-element-issues) to be present in each component. Simply put, you should always write code inside `<div>Your Code Here</div>`. Omitting this structure will lead to a lot of problems with updating components.

[Example](https://github.com/michael-rubel/livewire-best-practices/blob/main/Examples/root-element.md)

---
### ✨ The Golden rule of performant Livewire
```html
Don't pass large objects to Livewire components!
```

Avoid passing objects to the component's `mount` method or properties if possible. Use primitive types: strings, integers, arrays, etc. That's because Livewire serializes/deserializes your component's payload with each request to the server to share the state between the frontend & backend. If you need to work on objects, you can create them inside a method or computed property, and then return the result of processing as an array or paginated collection if needed.

What to consider a large object?
- Any instance large as Eloquent Model is big enough already for Livewire to slow down the component lifecycle, which may lead to poor performance on live updates. For example, if you have a component that represents the user profile (email and username), it's better to pass these parameters to properties as strings instead of the assignment of the whole model and extracting its attributes in the view.

Note: if you use [full-page components](https://laravel-livewire.com/docs/2.x/rendering-components#page-components), it's recommended to fetch objects in the full-page component itself, and then pass them downstairs to the nested ones as primitive types.

---
### 🧵 Keep component nesting level at 1
If you had a Livewire component (0) that includes another Livewire component (1), then you shouldn't nest it deeper (2+). Too much nesting can make a headache when dealing with DOM diffing issues.

Prefer the usage of Blade components when you do want to use nesting, they will be able to communicate with the parent's Livewire component.

[Example](https://github.com/michael-rubel/livewire-best-practices/blob/main/Examples/nesting-level.md)

---
### 🗺️ Use Route Model Binding to fetch the model
Pass only an ID or UUID to the `mount` method, then map the model attributes to component properties. Remember: don't assign a whole model, but its attributes only. To avoid manually mapping model attributes, you can use `fill` method, or [Loop Functions](https://github.com/michael-rubel/laravel-loop-functions#assign-eloquent-model-attributes-to-class-properties) package.

[Example](https://github.com/michael-rubel/livewire-best-practices/blob/main/Examples/route-model-binding.md)

---
### 💡 Use *debounce*, *lazy* & *defer* wire:model's modifiers
You should use [wire:model's modifiers](https://laravel-livewire.com/docs/2.x/properties#debouncing-input) based on requirements for a particular input.\
This will dramatically reduce unnecessary requests to the server.

[Example](https://github.com/michael-rubel/livewire-best-practices/blob/main/Examples/wire-model-modifiers.md)

---
### 🕵️ Don't pass sensitive data to the components
Avoid situations that may lead to passing sensitive data to the Livewire components, because they can be easily accessed from the client side by default. Always hide sensitive attributes of your models using `$hidden` property or explicitly filter the data you are fetching. You can as well make your properties `protected` in the Livewire component and then access it using `$this`, which will make them unavailable for the browser but accessible in Blade templates.

---
### ☔ Prefer to use event listeners over polling
Instead of constantly [polling](https://laravel-livewire.com/docs/2.x/polling#polling-background) the page to refresh your data, you may use [event listeners](https://laravel-livewire.com/docs/2.x/events#event-listeners) to perform the component update only after a specific task was initiated from another component.

[Example](https://github.com/michael-rubel/livewire-best-practices/blob/main/Examples/event-listeners-over-polling.md)

---
### 📦 Use computed properties to access database
You can use [computed properties](https://laravel-livewire.com/docs/2.x/properties#computed-properties) to avoid unnecessary database queries. Computed properties are cached within the component's lifecycle and do not perform additional SQL queries on subsequent requests when updating the state of an already mounted component.

[Example](https://github.com/michael-rubel/livewire-best-practices/blob/main/Examples/computed-properties.md)

---
### ➰ Keep track of a DOM elements
If you face problems with updating content in your components, you must consider using [`wire:key`](https://laravel-livewire.com/docs/2.x/troubleshooting#dom-diffing-cures) construction to tell Livewire how to keep track of your DOM elements. You will typically need this when you want to update something inside a loop or if you constantly poll the root component expecting updates in the nested ones. Remember to avoid using identical `wire:key` for multiple components, this may lead to unpredictable bugs.

[Example](https://github.com/michael-rubel/livewire-best-practices/blob/main/Examples/wire-key.md)

---
### 👨‍💻 Use Artisan commands to create, move and rename components
Livewire has [built-in Artisan commands](https://laravel-livewire.com/docs/2.x/reference#artisan-commands) to create, move, rename components, etc.
For example, instead of manually renaming files, which could be error-prone, you can use the following command:
- `php artisan livewire:move Old/Path/To/Component New/Path/To/Component`

---
### 💱 Always use loading states for better UX
You can use [loading states](https://laravel-livewire.com/docs/2.x/loading-states) to make UX better. It will indicate to the user that something is happening in the background if your process is running longer than expected. To avoid flickering, you can use the `delay` modifier.

[Example](https://github.com/michael-rubel/livewire-best-practices/blob/main/Examples/loading-states.md)

---
### 🔗 Entangle your live data
`wire:model` is bound to the server-side request to update the state of the component, but you can optionally use [AlpineJS](https://alpinejs.dev/) to add client-side reactivity. Instead of using `wire:model`, you can use `x-model` from AlpineJS and sync your data with the backend using [@entangle](https://laravel-livewire.com/docs/2.x/alpine-js#sharing-state) directive. This way the model will update instantly on the frontend, and the data would persist server-side after the network request reach the server. It dramatically improves the user experience on slow devices.

[Example](https://github.com/michael-rubel/livewire-best-practices/blob/main/Examples/entangle.md)

---
### 🌎 Use Form Request rules for validation
Livewire doesn't support [Form Requests](https://laravel.com/docs/9.x/validation#form-request-validation) internally, but instead of hardcoding the array of validation rules in the component, you may get it directly from Form Request.
This way you can reuse the same validation rules in different application layers, for example in API endpoints.

[Example](https://github.com/michael-rubel/livewire-best-practices/blob/main/Examples/form-request.md)

---
### 🧪 Always write feature tests
Even simple tests such as [this one](https://laravel-livewire.com/docs/2.x/testing#testing-passed-data) can help you a lot when you change something in the component.
Livewire has a very simple yet powerful [testing API](https://laravel-livewire.com/docs/2.x/testing#introduction). The [Missing Livewire Assertions](https://github.com/christophrumpel/missing-livewire-assertions) package may help you extend the set of available testing methods.

---
> 🔨 Are you working with Livewire on a daily basis? PRs are welcome.\
> Suggest your best practices if you don't see them on the list.\
> If you aren't sure if it's a good practice, you can [start a discussion](https://github.com/michael-rubel/livewire-best-practices/discussions/new).

## Contributors
[![GitHub Contributors Image](https://contrib.rocks/image?repo=michael-rubel/livewire-best-practices)](https://github.com/michael-rubel/livewire-best-practices/graphs/contributors)
