### ๐งต Keep component nesting level at 1

Example:
```html
<div> <!โโ level 0 โโ>
    <h1>Component</h1>
    <livewire:profile :user="auth()->user()->uuid" /> <!โโ level 1 โโ>
</div> 
```
