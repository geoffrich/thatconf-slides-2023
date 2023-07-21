
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

---

# waiting for the parent data

<!--
[ this is cuttable if pressed for time. can briefly explain in final part if needed ]

though sometimes you can't fully run in parallel and you have a page that needs data from a parent load function

you can use `await parent`

you want to be careful, because when you do this the rest of your load function will stop executing in parallel

if you're making other api calls, fire those off first (just like making api calls in parallel)

one example where this can be useful: favorites. all data loading is in layout, pages just extract data from that
-->

---

# invalidation: when do you re-run load functions?

- making a change to data on the backend 
  - user logging in
  - saving an album as a favorite
- navigating from one page to another
  - e.g. /album/1 => /album/2

<!--
[ TODO maybe cut / fold into form section or live demo? ]

we've only talked about when load functions run when a page is first rendered - you go to the album page, so call the load function to get data about that album

sometimes you need to re-run load functions because the data has changed. SK will try to do that intelligently

you want to be efficient about it without getting stale data: run too often and it's wasteful, don't run enough and data becomes out of date

SK will track what data is used in each load function and only re-run it when the data changes

let's look at an example: navigating between artists

when you first load the page, two load functions are run. the layout load and the page load

but then subsequent navigations will only run the page load. this is because that load uses the url (and params), so it will re-run it whenever the URL changes

however, the layout load does not depend on anything that is changing, so it only runs once

that's why it's efficient to get shared data in the layout, because it won't re-run on every page

however, sometimes you need to reload all the data. when you submit a form, that form submission could have changed anything on your backend. So SK will rerun all the load functions on the page by default

this isn't very efficient right? but it's a good default to keep data from getting stale. later we'll come back to ways to more intelligently rerun certain load functions
-->