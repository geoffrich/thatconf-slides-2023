---
theme: seriph
background: ""
class: "bg-gradient-to-r from-gray-100 to-gray-400 dark:from-gray-700 dark:to-gray-900 p-0"
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
transition: slide-left
title: SvelteKit - beyond the basics
---

# Web development, streamlined

## An introduction to SvelteKit

<img class="mt-20" src="/svelte-machine.png" />

---

# About me

- Svelte maintainer
- Senior Software Engineer at Ordergroove
- pianist
- cat lover

<br>

<div class="images">

<img src="/cats.jpg" height="300" width="200">

<img src="/cats2.jpg" height="200" width="300">

</div>

<style>
  .images {
    display: flex;
    align-items: center;
    gap: 2rem;
  }
</style>

---

# What is Svelte?

- component-based JS framework
- uses a compiler
- smaller & faster* apps

<style>
  ul {
    font-size: 1.5rem;
  }
</style>

<!--
- [ show of hands — who has heard of Svelte? used Svelte? ]
- So Svelte has been getting fairly well known these days, but in case this is your first time hearing about it — it is a component-based JavaScript framework like React and Vue, but the major difference is instead of interpreting your component code with a runtime it ships to the browser, it compiles your components into vanilla JavaScript at build time. So on average, this makes for applications that are typically smaller and faster than applications built with the other big frameworks.
-->

---

# Minimal boilerplate

```svelte {all|2,6|2,6,9-11|2,3,6,7|9-19|all}
<script>
  let count = 0;
  $: doubled = count * 2;
</script>

<p>The count is {count}</p>
<p>Doubled: {doubled}</p>

<button on:click={() => {
  count = count + 1;
}}>
  Increment
</button>

<style>
  button {
    padding: 0.5rem;
  }
</style>
```

<!--
But for me, the reason I love svelte is not because it's the smallest or the fastest framework -- it does well there, but it's not #1

It's how productive it feels to write Svelte components.

Because it’s a compiler, the component authoring experience can also be optimized for simplicity and ease of use

This is what a Svelte component looks like

- In a .svelte file
- State
- Updating state
- Reactive state
- Scoped styles

That's Svelte, if it's new to you I recommend giving it a try

Svelte is my favorite way to write UI components, but when building a web app you often need more than just a way to write components. That’s where SvelteKit comes in
-->

---

# What SvelteKit handles

<div class="grid grid-cols-2 gap-6 text-2xl">

- router
- data loading
- build optimization
- form handling
- SSR
- deployment
- and more!

<img src="/legos.jpg" width="300" height="200">

</div>


<!--
SvelteKit - everything you need to build a full app or website

handles the layer on top of your Svelte components 

So you don't have to provide answers for all these things yourself

"metaframework"

Svelte just says here’s a component language, it’s up to you to wire everything together, figure out how to deploy it. SvelteKit provides structure & conventions and lets you spend your time building instead of configuring 

it’s the Svelte team’s recommended way to build any app with Svelte.

Svelte is individual legos, SvelteKit helps assemble those legos into a full build
-->

---
class: bg-white
---

<div class="wrapper">

<img src="/nextjs-logo.png">
<img src="/remix-logo.png">
<img src="/nuxt-logo-big.png">

</div>

<style>
  .wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 2rem;
  }
  img {
    flex-shrink: 0;
    max-width: 450px;
  }
</style>


<!--
If you’re familiar with the React ecosystem, think of it like NextJS or Remix, except unlike those frameworks Svelte and SvelteKit are made by the same team.
-->

---

# Plan for today

- TODO update

<img src="/learn-svelte-dev.png" class="pt-10">

<!--
Will be doing a live demo to see it in action, but this isn’t the right place for a full tutorial. If you want that, learn.svelte.dev
-->

---

# Directory-based routing

folders create routes

```text {all|2-5|3|4-5|all}
src/
├─ routes/
│  ├─ +page.svelte
│  ├─ about/
│  │  ├─ +page.svelte
```

<br>

```html
<a href="/">Home</a>
<a href="/about">About</a>
```

<!--
Folders create routes

To know what renders at any given URL, you go to the corresponding folder

`<a>` link between them - no link component

by default, SvelteKit will server render each route for the initial request, for good SEO and so that users see content right away

and then after the initial load, SK will use client-side navigation for a snappy, app-like experience
-->

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

# Loading data: the load function

```text {all|3-4|3,5}
src/
├─ routes/
│  ├─ about/
│  │  ├─ +page.svelte
│  │  ├─ +page.server.js
```

<br>

<div class="grid grid-cols-2 gap-6">

<div>
+page.server.js

```js
export async function load() {
	const items = await api.getItems();
	return {
		items
	};
}
```
</div>

<div>
+page.svelte

```svelte
<script>
  export let data;
</script>

<ul>
{#each data.items as item}
  <li>{item}</li>
{/each}
</ul>
```
</div>

</div>

<!--
loads the data for the page

when you navigate to this route, SK will call the load function

available in the data prop
-->

---

# Mutating data: actions

<div class="grid grid-cols-2 gap-6">

<div>
+page.svelte

```svelte {1-5|6-23}
<form method="POST" action="/?login">
  <label>Username <input type="text" name="username"></label>
  <label>Password <input type="password" name="password"></label>
  <button>Log in</button>
</form>

<script>
  import { enhance } from '$app/forms';
</script>

<form method="POST" action="/?login" use:enhance>
  <label>Username <input type="text" name="username"></label>
  <label>Password <input type="password" name="password"></label>
  <button>Log in</button>
</form>
```
</div>

<div>
+page.server.js

```js
export const actions = {
  login: async ({ request }) => {
    // log the user in
    const data = await request.formData();
    const username = data.get('username');
    const password = data.get('password');
    await performLogin({ username, password });
  }
}
```

<br>

<div v-click>

- with `use:enhance`, the form will submit without reloading the page
- can pass a callback to customize the behavior

</div>

</div>

</div>

<!--
just like most apps need to load data, most apps need to mutate data

form has seen a bit of a comeback lately in the JS framework world (Remix, Next, Solid Start)

SK also likes forms

- progressive enhancement
-->

---
layout: image
image: /everyone-has-js.png
---

https://www.kryogenix.org/code/browser/everyonehasjs.html

<style>
  .slidev-layout {
    /* https://github.com/slidevjs/slidev/issues/798 */
    background-size: contain !important;
  }

  a {
    position: absolute;
    bottom: 0.5rem;
    @apply bg-black;
  }
</style>

<!--
Everyone has JS, right? No

very few people disable JS. but it's common for people to be in situations where JS is slow to load or even fails to load

If all your logic is in JS, if your JS fails to load, then you're out of luck

But if your app is SvelteKit (or another framework that prioritizes PE), the defaults will ensure some functionality (SSR, links to navigate, forms submit data)
-->

---

# Where to next?

- the docs: svelte.dev / kit.svelte.dev
- the tutorial: learn.svelte.dev
- Svelte discord: svelte.dev/chat

## my stuff

<br>

- site: geoffrich.net
- twitter: @geoffrich_
- mastodon: @geoffrich@front-end.social

---
layout: fact
---

# Thanks!
