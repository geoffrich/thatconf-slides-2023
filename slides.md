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

# Building _efficient_ and _resilient_ web apps with SvelteKit

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

# What this talk is

- building **efficient** and **resilient** web apps
- a bit of a grab bag
- moving beyond the basics of SvelteKit (w/ a quick recap)
- practical examples - "Sveltunes"

---
layout: two-cols
---

# efficiency

- only fetch what you need
- fetch in parallel
- prefetch when you can
- avoid re-fetching
- fetch at build time
- minimal boilerplate (efficiency while authoring)
- ship less JS

::right::

# resiliency

- JS is an enhancement
- building on the web platform (links & forms)
- typesafety
- prerendering

<!--
TODO should I talk about all this up front?
-->

---
layout: center
---

## previously, on

<div class="relative">

<img class="svelte" width="300" src="/svelte-logo.svg">

<img src="/arrested-development.jpg">

</div>

<svg class="w-full h-full absolute top-0 left-1 touch-none"><path fill="#ff595e" d="M 262.67 250.67 Q 262.67 250.67 267.89 248.68 273.11 246.68 279.17 246.68 285.23 246.68 291.93 246.68 298.62 246.68 307.51 246.68 316.39 246.68 328.96 246.68 341.52 246.68 356.48 246.68 371.43 246.68 385.47 246.68 399.51 246.68 412.90 246.68 426.28 246.68 438.56 246.68 450.84 246.68 461.00 246.68 471.16 246.68 480.42 246.68 489.68 246.68 497.46 246.68 505.24 246.68 511.02 246.68 516.80 246.68 522.28 246.68 527.75 246.68 532.55 246.68 537.35 246.68 541.31 246.68 545.27 246.68 548.61 246.68 551.94 246.68 555.00 246.68 558.06 246.68 560.46 246.68 562.86 246.68 565.26 246.68 567.65 246.68 570.04 246.68 572.43 246.68 574.82 246.68 577.21 246.68 579.32 246.68 581.44 246.68 583.44 246.68 585.44 246.68 587.43 246.98 589.42 247.29 592.75 247.73 596.07 248.18 599.78 248.65 603.49 249.12 607.01 249.29 610.53 249.46 612.68 249.79 614.84 250.11 617.09 250.21 619.34 250.32 622.17 250.66 625.00 251.00 627.65 251.13 630.30 251.26 633.27 251.60 636.24 251.95 639.91 252.36 643.57 252.78 647.74 253.52 651.90 254.27 655.25 254.85 658.60 255.43 661.92 255.95 665.23 256.47 668.56 256.96 671.90 257.45 675.07 257.65 678.25 257.84 681.43 258.22 684.61 258.59 687.61 259.03 690.62 259.46 693.10 259.63 695.57 259.80 697.99 259.87 700.40 259.94 702.80 259.98 705.19 260.01 707.31 260.02 709.43 260.03 712.71 260.04 715.99 260.05 719.32 260.05 722.65 260.05 725.21 260.06 727.76 260.06 730.19 260.06 732.61 260.06 735.69 260.46 738.78 260.86 741.29 260.93 743.80 261.00 746.66 261.45 749.53 261.89 753.00 263.93 756.47 265.96 756.47 265.96 756.47 265.97 756.47 265.97 756.47 265.98 754.62 267.92 752.77 269.87 750.47 269.79 748.17 269.70 745.94 269.35 743.71 269.00 740.87 268.91 738.03 268.82 735.31 268.44 732.59 268.05 730.18 268.06 727.76 268.06 725.20 268.06 722.65 268.05 719.31 268.05 715.97 268.05 712.67 268.04 709.38 268.03 707.23 268.02 705.08 268.01 702.62 267.97 700.15 267.94 697.59 267.86 695.02 267.78 692.24 267.58 689.47 267.38 686.57 266.96 683.67 266.53 680.71 266.18 677.75 265.83 674.24 265.60 670.72 265.37 667.36 264.87 664.00 264.37 660.62 263.84 657.23 263.31 653.86 262.73 650.48 262.14 646.58 261.44 642.67 260.73 638.99 260.31 635.30 259.89 632.60 259.57 629.90 259.25 626.97 259.10 624.03 258.94 621.48 258.63 618.93 258.31 616.26 258.16 613.60 258.01 609.94 257.67 606.29 257.33 602.51 256.85 598.73 256.38 596.68 256.21 594.62 256.05 591.37 255.61 588.13 255.18 584.78 254.93 581.44 254.68 579.32 254.68 577.21 254.68 574.82 254.68 572.43 254.68 570.04 254.68 567.65 254.68 565.26 254.68 562.86 254.68 560.46 254.68 558.06 254.68 555.00 254.68 551.94 254.68 548.61 254.68 545.27 254.68 541.31 254.68 537.35 254.68 532.55 254.68 527.75 254.68 522.28 254.68 516.80 254.68 511.02 254.68 505.24 254.68 497.46 254.68 489.68 254.68 480.42 254.68 471.16 254.68 461.00 254.68 450.84 254.68 438.56 254.68 426.28 254.68 412.90 254.68 399.51 254.68 385.47 254.68 371.43 254.68 356.48 254.68 341.52 254.68 328.96 254.68 316.39 254.68 307.51 254.68 298.62 254.68 291.93 254.68 285.23 254.68 279.17 254.68 273.11 254.68 267.89 252.69 262.67 250.69 262.67 250.68 Z"></path></svg>

<style>
  .svelte {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    top: 50px;
  }

</style>


<!--
Svelte/Svelte

but if you're here and are completely new to Svelte or SvelteKit, quick recap
-->

---

# What is Svelte?

- component-based JS framework
- uses a compiler
- smaller & faster apps

<style>
  ul {
    font-size: 1.5rem;
  }
</style>

<!--
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

today we're going to focus a lot on data loading and form handling
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
layout: fact
---

# What are the SvelteKit basics?

<!--
let's level-set on what SvelteKit provides
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
TODO this examples should use Sveltunes

Folders create routes

To know what renders at any given URL, you go to the corresponding folder

`<a>` link between them - no link component

by default, SvelteKit will server render each route for the initial request, for good SEO and so that users see content right away

and then after the initial load, SK will use client-side navigation for a snappy, app-like experience
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

this ties into _resiliency_, will talk more later
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
