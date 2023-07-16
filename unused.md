
---

# Loading data: before SvelteKit

```svelte
<script>
  import { onMount } from 'svelte';

  let data;

  onMount(() => {
    fetch('/api/items')
      .then(r => r.json())
      .then(result => {
        data = result;
      });
  });
</script>
```

<br>

<v-clicks>

- doesn't start until JS parsed & component rendered
- boilerplate
- users without JS are out of luck

</v-clicks>

<!--
most apps need to load data - here's how you'd do that in pure svelte

parsing/loading/error handling all by yourself

issues with this approach

this still works in SvelteKit but there's a better way
-->
