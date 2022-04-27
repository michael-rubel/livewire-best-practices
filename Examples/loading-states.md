### 💱 Always use loading states for better UX

Example:
```html
<div>
    <button wire:click="update">Update</button>

    <span wire:loading.delay wire:target="update">
        Loading...
    </span>
</div>
```