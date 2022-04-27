### ➰ Keep track of a DOM elements

Example:
```html
<div>
    <h1>Component</h1>
    <livewire:chart
        :wire:key="$chart->uuid"
        :chart="$chart->uuid"
    />
</div> 
```

So the component will be updated if we add a new record of the chart data (UUID changes).