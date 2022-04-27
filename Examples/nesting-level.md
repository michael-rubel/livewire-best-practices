### 🧵 Keep component nesting level at 1

Example:
```html
<div> <!–– level 0 ––>
    <h1>Component</h1>
    <livewire:profile :user="auth()->user()->uuid" /> <!–– level 1 ––>
</div> 
```
