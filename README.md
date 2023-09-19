![Livewire Best Practices](https://avatars.githubusercontent.com/u/51960834?s=100&v=4)

## Livewire Best Practices
[![StandWithUkraine](https://raw.githubusercontent.com/vshymanskyy/StandWithUkraine/main/badges/StandWithUkraine.svg)](https://github.com/vshymanskyy/StandWithUkraine/blob/main/docs/README.md)

This repository is a curated list of general recommendations on how to use [Laravel Livewire framework](https://github.com/livewire/livewire) to meet enterprise concerns regarding security, performance, and maintenance of Livewire components.

### Short introduction
My name is [Michael Rubél](https://github.com/michael-rubel) and I started using the Livewire framework in 2019 when it was new and barely stable. Back in the day, I was impressed with how fast dynamic UIs can be shipped without even using JavaScript. But like any software solution, it had its pitfalls, and I had to deal with them. The main goal of this repository is to collect the most important experiences that you need to consider when working with Livewire.

Let's begin...

---
### 🌳 Always set up root element
Livewire requires a root element (div) to be present in each component. Simply put, you should always write code inside `<div>Your Code Here</div>`. Omitting this structure will lead to a lot of problems with updating components.

[Example](https://github.com/michael-rubel/livewire-best-practices/blob/main/Examples/root-element.md)

---
### ✨ The Golden rule of performant Livewire
```html
Don't pass large objects to Livewire components!
```

Avoid passing objects to the component's public properties if possible. Use primitive types: strings, integers, arrays, etc. That's because Livewire serializes/deserializes your component's payload with each request to the server to share the state between the frontend & backend. If you need to work on objects, you can create them inside a method or computed property, and then return the result of the processing.

What to consider a large object?
- Any instance large as Eloquent Model is big enough already for Livewire to slow down the component lifecycle, which may lead to poor performance on live updates. For example, if you have a component that represents the user profile (email and username), it's better to pass these parameters to properties as strings instead of the assignment of the whole model.

Note: if you use [full-page components](https://livewire.laravel.com/docs/components#full-page-components), it's recommended to fetch objects in the full-page component itself, and then pass them downstairs to the nested ones as primitive types.

---
### 🧵 Keep component nesting level at 1
If you had a Livewire component (0) that includes another Livewire component (1), then you shouldn't nest it deeper (2+). Too much nesting can make a headache when dealing with DOM diffing issues.

Also, prefer the usage of Blade components when you use nesting, they will be able to communicate with the parent's Livewire component but won't have the overhead the Livewire adds.

[Example](https://github.com/michael-rubel/livewire-best-practices/blob/main/Examples/nesting-level.md)

---
### 🗺️ Use Route Model Binding to fetch the model
Pass only an ID or UUID to the `mount` method, then map the model attributes to component properties. Remember: don't assign a whole model, but its attributes only. To avoid manually mapping model attributes, you can use `fill` method, or [Loop Functions](https://github.com/michael-rubel/laravel-loop-functions#assign-eloquent-model-attributes-to-class-properties) package.

[Example](https://github.com/michael-rubel/livewire-best-practices/blob/main/Examples/route-model-binding.md)

---
### 💡 Avoid using *live* wire:model modifier where possible
Avoid using `live` wire:model modifier. This dramatically reduces unnecessary requests to the server.
In Livewire version 3, all the models are deferred by default (old: `defer` modifier), which is good.

[Examples](https://github.com/michael-rubel/livewire-best-practices/blob/main/Examples/wire-model-modifiers.md)

---
### 🕵️ Don't pass sensitive data to the components
Avoid situations that may lead to passing sensitive data to the Livewire components, because it can be easily accessed from the client-side by default. You can hide the properties from the frontend using `#[Locked]` attribute starting from Livewire version 3.

---
### ☔ Prefer to use event listeners over polling
Instead of constantly [polling](https://livewire.laravel.com/docs/polling) the page to refresh your data, you may use [event listeners](https://livewire.laravel.com/docs/events#listening-for-events) to perform the component update only after a specific task was initiated from another component.

[Example](https://github.com/michael-rubel/livewire-best-practices/blob/main/Examples/event-listeners-over-polling.md)

---
### 📦 Use computed properties to access database
You can use [computed properties](https://livewire.laravel.com/docs/computed-properties) to avoid unnecessary database queries. Computed properties are cached within the component's lifecycle and do not perform additional SQL queries on multiple calls in the component class or in the blade view.

[Example](https://github.com/michael-rubel/livewire-best-practices/blob/main/Examples/computed-properties.md)

---
### 👨‍💻 Use Artisan commands to create, move and rename components
Livewire has [built-in Artisan commands](https://livewire.laravel.com/docs/quickstart#create-a-livewire-component) to create, move, rename components, etc.
For example, instead of manually renaming files, which could be error-prone, you can use the following command:
- `php artisan livewire:move Old/Path/To/Component New/Path/To/Component`

---
### 💱 Always use loading states for better UX
You can use [loading states](https://livewire.laravel.com/docs/loading#basic-usage) to make UX better. It will indicate to the user that something is happening in the background if your process is running longer than expected. To avoid flickering, you can use the `delay` modifier.

[Example](https://github.com/michael-rubel/livewire-best-practices/blob/main/Examples/loading-states.md)

---
### 📈 Use lazy loading
Instead of blocking the page render until your data is ready, you can create a placeholder using the [lazy loading](https://livewire.laravel.com/docs/lazy) technique so your UI will feel more responsive.

[Example](https://github.com/michael-rubel/livewire-best-practices/blob/main/Examples/lazy-loading.md)

---
### 🔗 Entangle
You can sync your data with the backend using [@entangle](https://livewire.laravel.com/docs/upgrading#entangle) directive. This way the model will be updated instantly on the frontend, and the data would persist server-side after the network request reach the server. It dramatically improves the user experience on slow devices.

[Example](https://github.com/michael-rubel/livewire-best-practices/blob/main/Examples/entangle.md)

---
### 🌎 Use Form Request rules for validation
Livewire doesn't support [Form Requests](https://laravel.com/docs/9.x/validation#form-request-validation) internally, but instead of hardcoding the array of validation rules in the component, you may get it directly from Form Request.
This way you can reuse the same validation rules in different application layers, for example in API endpoints.

[Example](https://github.com/michael-rubel/livewire-best-practices/blob/main/Examples/form-request.md)

---
### 🧪 Always write feature tests
Even simple tests can help you a lot when you change something in the component.
Livewire has a very simple yet powerful [testing API](https://livewire.laravel.com/docs/testing).

---
> 🔨 Are you working with Livewire on a daily basis? PRs are welcome.\
> Suggest your best practices if you don't see them on the list.\
> If you aren't sure if it's a good practice, you can [start a discussion](https://github.com/michael-rubel/livewire-best-practices/discussions/new).

## Contributors
[![GitHub Contributors Image](https://contrib.rocks/image?repo=michael-rubel/livewire-best-practices)](https://github.com/michael-rubel/livewire-best-practices/graphs/contributors)
