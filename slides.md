---
theme: ./theme
background: ''
class: >-
  bg-gradient-to-r from-gray-100 to-gray-400 dark:from-gray-700 dark:to-gray-900
  p-0
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
transition: slide-left
title: Building efficient and resilient web apps with SvelteKit
---

# Building _efficient_ and _resilient_ web apps with SvelteKit

Geoff Rich

<img class="mt-20" src="/svelte-machine.png" />

<!--
this talk is a bit of a grab bag, but focused on two characteristics of well-built web apps

1. they're efficient and aren't wasteful in the amount of data they load and the time it takes to load that data
2. they're resilient, especially if your app's JS is slow to load or fails entirely 

and we're going to focus on how to build apps with SvelteKit that meet these criteria

I also want to go somewhat beyond the basics of SvelteKit -- this talk was originally titled "Intermediate SvelteKit." I will be recapping those basics for any of you who are less familiar with Svelte or SvelteKit, but I want to go beyond SvelteKit's "hello world" and focus on more intermediate features.
-->

---

# About me

- Svelte maintainer
- Senior Software Engineer at Ordergroove
- pianist & cat lover

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
[ show of hands — who has written a Svelte component? what about SvelteKit? ]

if you're here and are completely new to Svelte or SvelteKit, quick recap
-->

---

# What is Svelte?

a _component-based JS framework_ that uses a <span class="svelte">compiler</span>

<style>
  h1 {
    text-align: center;
    font-size: 3rem !important;
  }

  p {
    text-align: center;
    display: block;
    margin-top: 7rem !important;
    font-size: 3rem !important;
    line-height: 1.5 !important;
    opacity: 1 !important;
  }

  .svelte {
    background-color: hsl(15, 100%, 40%);
    color: white;
    padding: 0.2rem 0.6rem;
    font-weight: bold;
  }
</style>

<!--
So Svelte has been getting fairly well known these days, but in case this is your first time hearing about it — it is a component-based JavaScript framework like React and Vue, but the major difference is instead of interpreting your component code with a runtime it ships to the browser, it compiles your components into vanilla JavaScript at build time. So the advantage of this is that you can ship less JavaScript to your users, and your application has to do less work because it has compiled away a lot at build time
-->

---

```svelte {all|2,6|2,6,9-11|2,3,6,7|9-19|all} {maxHeight:'500px'}
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
- Scoped styles (batteries included)

That's Svelte, if it's new to you I recommend giving it a try

Svelte is my favorite way to write UI components, but when building a web app you often need more than just a way to write components.
-->

---

# Production app requirements

<v-clicks>

- router
- data loading
- build optimization
- form handling
- server-side rendering (SSR)
- deployment
- and more!

</v-clicks>

<!--
There are all these things that go into building a full web app that a component framework doesn't handle

while you can answer all these yourself or find packages on npm, it would be nice to have a framework that already answers these questions so you don't have to

and that's what SvelteKit does
-->

---
layout: center
---

<img src="/sveltekit-logo.png">

<!--

SvelteKit brings everything you need to build a full app or website

Svelte just says here’s a component language, it’s up to you to wire everything together, figure out how to deploy it. SvelteKit provides structure & conventions and lets you spend your time building instead of configuring and choosing between packages on npm

it’s the Svelte team’s recommended way to build any app with Svelte.

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
If you’re familiar with the React ecosystem, think of it like NextJS or Remix
-->

---
layout: fact
---

# introducing Sveltunes

<!--
Before we get too far, let's see what a SvelteKit app looks like

I want to keep this practical, so a lot of the concepts we talk about today will be filtered through this music library demo app I built, Sveltunes

to highlight:
- you can do a few things: search for artists and albums, view details, and "favorite" albums to save then to your library
- by default, the page is server rendered, so the browser can start showing content right away
- but after the initial request, client-side routing takes over, so you get snappy, instant navigation
- part of that fast navigation is due to preloading, where SK will start fetching the data for the next page as the user hovers over a link, so when they do click the link that navigation will happen instantly
- and then the data updates in this app all use enhanced forms, so they'll still work when JS fails but provide an enhanced UX when JS is available
- we sometimes call apps built like this "transitional apps", because they're not a multi-page app that's only server-rendered, or a single-page app that only works with JS. it's a hybrid approach that brings the best of both worlds
-->

---
layout: fact
---

# efficiency and resiliency

<!--
today I want to talk about building apps with SK, and specifically building apps that are efficient and resilient

let's start with efficiency
-->

---
layout: fact
---

# efficiency: minimizing waste

<!--
to me, efficiency means minimizing waste

and that means building web apps that respect our users time, that retrieve data quickly and don't ask for more data unnecessarily

but while I'll mostly focus on building an efficient data loading experience, developing with SK is also an efficient experience, since it prevents you the developer from wasting time
-->

---

# efficient developer experience (DX)

- writing Svelte components
  - `let count = 3`
  - `$: doubled = count * 2`
- directory-based routing

<!--
when you're writing Svelte/SvelteKit, the code you write is efficient to read and efficient to author

Svelte components: state is a variable that you read and update, $: syntax

directory based routing in SvelteKit [ next slide ]
-->

---

# Directory-based routing

folders create routes

```text {all|2-5|3|4-5|all}
src/
├─ routes/
│  ├─ +page.svelte
│  ├─ search/
│  │  ├─ +page.svelte
```

<!--
Folders create routes

efficient: To know what renders at any given URL, you go to the corresponding folder
-->

---

# efficient JS payloads

- code splitting
- minimizing bundle size

<!--
but back to the user experience, SK takes steps to ship efficient JS payloads

code-splitting: code will be split between routes, depending on usage

Svelte components have pretty minimal bundle size, so your average SvelteKit app is going to be much smaller than your average Next or Remix app
-->


---
layout: fact
---

# efficient data loading

<!--
where I want to spend most time

because most of your apps loading or transition time is spent doing data loading

if the content is static, it's quick. it's only when you need to call an API or query a DB that things can slow down.

so if we can improve how your app loads data, then we can improve the efficiency of your app overall

in SvelteKit, data loading occurs in the load function
-->

---

# Loading data: the load function

<br>

<div class="grid grid-cols-2 gap-6">

<div v-click="1">
+page.server.js

```js {all|all|2-6}
export async function load() {
	const detail = 
      await api.getAlbumInfo();
	return {
		detail
	};
}
```
</div>

<div v-click="3">
+page.svelte

```svelte {all|2|2,5-6}
<script>
  export let data;
</script>

<h1>{data.detail.title}</h1>
<p>{data.detail.description}
```
</div>

</div>

<!--
if you've used SK before, you've almost certainly seen the load function

the load function is the recommended way to load data for the page in SK

how it works is you export a load function in +page.server.js file

when you navigate to this route, SK will call the load function and use the data it returns

let's say you have a page showing information about a specific music album - load function might look like this

then use the result to render the new page 

available in the data prop

and SK didn't introduce the load function arbitrarily. it's more efficient that what most Svelte apps were doing before SK
-->

---

# Loading data: before SvelteKit

```svelte
<script>
  import { onMount } from 'svelte';

  let data;
  onMount(() => {
    data = await api.getAlbumInfo();
  });
</script>
```

<!--
compare this to how you'd do it in pure Svelte

similar amount of code, but this will usually be less efficient

that's because the load function is able to start fetching data a lot sooner

while the load function can start fetching data on the server, this fetch call won't happen until after the JS for the page has loaded and this component has fully mounted

and that's because the load function's biggest advantage is that it's discoverable
-->

---

# the `load` function is _discoverable_

This means the framework can easily identify what data needs to load for a given page.

<v-clicks>

- starts sooner
- SSR
- preloading on hover for faster navigations

</v-clicks>

<v-click>

Disadvantage: it's only at the page level

</v-click>

<!--
a load function is just an export from a file in the routes tree, so SK can easily identify what load functions need to run for a given navigation

whereas for onMount, you need to render the whole component first, so SK can't start loading the data until much later

[ explain benefits ]

disadvantage: can't be component level. load function have to be associated with routes. sometimes you might want to encapsulate data loading inside a component, and then you'll have to reach for onMount and the associated tradeoffs

but load handles the majority of cases well, and that's what we'll be focusing on today

so that's why load is more efficient. but the load function also has a lot more features that help you deliver an efficient experience
-->

---

# layout loads

what do you do with data that is needed by multiple pages in your app?

<img src="/layout-load.png">

<!--

page load function => loads data needed by that page

what if you have data that is needed by multiple pages?

example: login state 
- nav needs it to show login/favorites links
- each album needs it to determine whether to show favorite button

well, layouts can have load functions too!

in a SK app, you can have layout components that contain shared UI for your app. e.g. in sveltunes, the nav bar is in a layout

so just like layouts reduce UI duplication, they can also reduce data loading duplication

just like you can create a +page.server.js with a load function, you can also create a +layout.server.js

and the data loaded by that layout is available across your app


[ demonstrate in sveltunes -- isLoggedIn set in layout load, but accessible in both layout and page ]

and while in this case we're not fetching the login state from anywhere, if the shared data you want to retrieve does involve a network call, fetching it in the layout can be even more valuable

you don't need to make duplicate calls, or try to introduce some sort of caching -- just fetch it in the layout and the pages can access it too

we'll see an example of this later

-->

---

# loading data in parallel (app-level)

<v-click>

```text {all|3,5}
src/
├─ routes/
│  ├─ +layout.server.js 🔴
│  ├─ album/[id]/
│  │  ├─ +page.server.js 🔵
│  │  ├─ +page.svelte
```

</v-click>
<v-click>

## sequential

<div class="sequence mb-3 mt-1">
  <div class="bar bar-1">4s</div>
  <div class="bar bar-2">5s</div>
  <div class="result">= 9s total</div>
</div>

</v-click>
<v-click>

## parallel

<div class="parallel mt-1">
  <div class="bar bar-3">4s</div>
  <div class="bar bar-4">5s</div>
  <div class="result">= 5s total</div>
</div>

</v-click>

<style>
  .sequence {
    display: grid;
    grid-template-columns: repeat(15, 20px);
    grid-template-rows: 18px;
    gap: 5px;
  }

  .parallel {
    display: grid;
    grid-template-columns: repeat(15, 20px);
    grid-template-rows: 18px 18px;
    gap: 5px;
  }

  .bar-1, .bar-3 {
    background: red;
  }

  .bar-2, .bar-4 {
    background: blue;
  }

  .bar {
    line-height: 1;
    color: white;
    padding-left: 0.25rem;
  }

  .result {
    font-size: 1.5rem;
    grid-column: 10 / span 5;
  }

  .bar-1 {
    grid-column: 1 / span 4;
  }

  .bar-2 {
    grid-column: 5 / span 5;
  }

  .bar-3 {
    grid-column: 1 / span 4;
  }

  .bar-4 {
    grid-column: 1 / span 5;
    grid-row: 2;
  }
</style>

<!--
so now we have potentially multiple load functions for a route - the load for the layout and the load for the page

and you can nest layouts in SK, so you could potentially have more

important to understand that SvelteKit doesn't run these load function sequentially (layout load => page load)

instead, they're run in parallel

which is good: each load function is independent, they don't need to wait for each other

the data your layout is loading isn't needed to fetch the data your album page is loading, so why wait?
-->

---

# loading data in parallel (page-level)

```js {all|2-4}
export function load({ params }) {
  // ❌ don't do this!
  const isFavorited = await api.isAlbumFavorited(params.id);
  const albumDetail = await api.getAlbumDetail(params.id);
  return {
    isFavorited,
    albumDetail
  }
}
```

<!--
this is also a principle you should follow inside single load functions

on the album page: favorite status and album info are two separate api calls. instead of awaiting one and then the other, fire them both at the same time
-->

---

# loading data in parallel (page-level)

```js {all|2-4|5-8}
export function load({ params }) {
  // ✅ do this!
  const isFavorited = api.isAlbumFavorited(params.id);
  const albumDetail = api.getAlbumDetail(params.id);
  return {
    isFavorited,
    albumDetail
  }
}
```

<!--
SK will automatically await top level promises
-->

---

# dealing with slow data

<div class="grid grid-cols-2 gap-6">

<div v-click>

```js
export function load() {
  const slowApiCall = 
      getSlowData();
  return {
    slow: {
      slowApiCall
    }
  }
}
```

</div>
<div v-click>

```svelte
{#await data.slow.slowApiCall}
<p>Loading...</p>
{:then result}
<p>Result: {result}</p>
{:catch e}
<p>Something went wrong</p>
{/await}
```

</div>

</div>

<!--
by default, SvelteKit won't navigate to a page until all the data has finished loading

but sometimes, your data can be slow

example: artists page. the call to get the list of albums is very slow. and this can really make your navigation feel sluggish

and it's not just client-side navigations - SK needs all the data for the page to server-side render it. which means if this is the first page your user visits, they might wait a while to see anything

normally your data sources should be fast enough that this won't be a problem. but in case you have slow data, SK has a way to render the page without waiting for it to load: streamed promises

remember how if we return promises, SK will automatically await them in parallel? well, that's only true at the top level. if the promise is nested, SK will not wait for it to resolve and return an actual Promise to the page

and that means we can show a loading state instead of blocking navigation

this will work on SSR too

let's see an example [ show artist page example ]

really powerful if you have slow data

reliant on JS though, so only use for non-essential data

lets you be more efficient - show the data you already have, stream in the rest
-->

---

# universal load functions (+page.js)

<v-clicks>

- can call external APIs directly
- can return non-serializable data

</v-clicks>

<v-click>

```js
import Component from './Comp.svelte';
import { writable } from 'svelte/store';
export function load({ data }) {
  return {
    component: Component,
    fn: () => "I can't be serialized",
    store: writable(data.whatever)
  }
}
```

</v-click>

<!--
All the load functions we've seen up to this point have been in .server.js files. This means they only run on the server, serialize the result, and return to the client

this is great since you can use sensitive env variables and access your databases and private APIs without exposing that to the client

You can also have what's called "universal" load functions in +page.js

1. these run on the server when retrieving data for the first server-side render
2. but then they only run on the client

and this has some advantages
- if you're calling external public APIs, you can call them directly from the browser instead of first making a call to your server (efficient!)
- they can return data that isn't serializable

we've seen that server load functions can return more than JSON - we were able to return Promises, and you're also able to return things like Maps and Sets. but there's a limit

if you want to return data that includes things like components or functions, you're going to need to use a universal load function. those run on the client instead of passing data over the network, so they can return whatever you want

and you can actually have both server and universal loads if you want to. the universal load gets the return value from the server load in data, and can either forward it along or use it to return different data

here's an example of a universal load function returning things you can't return from a server load function

this was a bit abstract now, but returning a store from a load function will be important later
-->

---

# Efficiency recap

<v-clicks>

- use the load function to start the request as soon as possible
  - preloading
- use layout loads for shared data
- load data in parallel
- stream slow data
- call APIs directly from client when possible

</v-clicks>

<!--

Recap of where we've been

later we'll talk about how efficiency intersects with forms, but first...

-->

---
layout: fact
---

# Resiliency

<!--
the ability to withstand difficult conditions

Resiliency can take many forms
-->

---
layout: fact
---

# what happens when JavaScript is not available?

<!--
notice I didn't say disabled - that can be why JS is not available, but it's not the only reason
-->

---

# when JS is not available

<v-clicks>

- the user disabled JS
- the page isn't finished loading (airport wifi?)
- the HTTP request failed
- the corporate firewall blocked it
- data saver mode
- browser extension interference
- and more

</v-clicks>

<v-click>

https://www.kryogenix.org/code/browser/everyonehasjs.html

</v-click>

<!--
Everyone has JS, right? No

it's not just the user disabled JS. it's common for people to be in situations where JS is slow to load or even fails to load

the web can be a hostile environment - Some studies show that JS fails on 3% of page loads, which is a lot

If all your logic is in JS, if your JS fails to load, then you're out of luck (white screen of death)

But if you build your app in a way that progressively enhances, where it's still functional as HTML, then parts of your app will still function
-->

---

# defaults & conventions

<v-clicks>

- SSR (returning real HTML)
  - using the load function
- links to navigate
- forms to submit data

</v-clicks>

<v-click>

(defaults, not demands)

</v-click>

<!--
so that's why SK encourages building your apps that way

defaults are important, since most devs won't change the defaults. you want the right thing to be the easy thing.

and in SK, the defaults are those that let you build a progressively enhanced experience, so your app doesn't completely white-screen in those 3% of cases where JS fails

without SSR, your app is 100% reliant on JS. and maybe that's fine for your usecase

everything we talked about how using the load function makes your app more efficient -- it also makes it more resilient, because data returned by the load function is able to be used for SSR


these are defaults, not demands - you can opt-out if you want to

but by using them, you make your app more resilient, because the browser knows how to work with these without loading all of your app's JS

the user can see your content and interact with it even when your JS is slow to load or fails entirely

100% of your app doesn't need to work without JS. that's impossible. but just getting the basics working can go a long way
-->

---
layout: fact
---

# _enhanced_ links and forms

<!--
and more importantly, these aren't just regular links and forms - they're enhanced

when JS is available, links will use client side routing instead of a full page reload and can preload the data for a navigation before the user actually clicks on the link

and with one attribute, forms will also be enhanced
-->

---

```svelte {all|4}
<form 
    method="POST" 
    action="/?login"
    use:enhance>
  <label>Username 
    <input 
      type="text" 
      name="username"></label>
  <label>Password 
    <input 
      type="password" 
      name="password"></label>
  <button>Log in</button>
</form>
```

<!--
normally, submitting a form completely reloads the page.

but if you add use:enhance, SvelteKit will emulate the browser-native behavior without the reload

which means you're free to enhance the experience with JS, for example adding animated transitions or optimistic UI

so you can write data updates in your app in a way that will work when JS is not available, but enhance that experience when it is available

the actual request handling happens inside page.server.js where you can read the submitted form data and respond to it. I'm going to focus on the client side enhancement for now. if you want to know more, look up "form actions" in the SK docs
-->

---

# Customizing `use:enhance`

```svelte
<form 
    method="POST" 
    action="/?login"
    use:enhance={() => {
      // do something before the submission happens
      return () => {
        // do something after the submission happens
      }
    }}></form>
```


<!--
you can get an enhanced form with just one attribute. but if you want more control over what happens with the form submission, you can pass a callback
-->

---

# Example: optimistic favorite UI

<v-click>

```svelte {all|15-21|5-9} {maxHeight:'470px'}
<script>
  export let data;
  let isSubmitting = false;

  // when submitting, assume the submission has succeeded 
  // and the value is flipped
  $: isFavorite = isSubmitting ? 
      !data.isFavorite : 
      data.isFavorite;
</script>

<form
	action={isFavorite ? '?/unfavorite' : '?/favorite'}
	method="POST"
	use:enhance={(event) => {
		isSubmitting = true;
		return async ({ update, result }) => {
			await update();
			isSubmitting = false;
		};
	}}
>
  <FavoriteButton {isFavorite} />
</form>
```

</v-click>

<!--
here's one way to customize enhance to get optimistic UI

normally: you click on a button, don't get feedback until the request completes

[ demo the experience on sveltunes ]

what if we assume it succeeds? change the state before the request happens

-->

---

# resilient _and_ efficient

- using forms, so our app is resilient
- but all the data is reloaded after submission, which is not efficient

<img src="/elephant.jpg" class="mt-4" width="500">

<!--
so we made our app more resilient using forms, but we've also made it less efficient

when you submit a form, that form submission could have changed anything on your backend. So SK will rerun all the load functions on the page by default

it's a good default to keep data from getting stale, but it's also inefficient - we could fetch data we don't actually need

take the favorite button example - when you add/remove a favorite, SK refetches all the data for the page by default - including the album details, which haven't changed

and this isn't efficient - we're wasting effort to load data that hasn't changed. a couple things to note:

1. this happens because it's a safe default. SK doesn't know that submitting the form only changes favorite state. all it knows is that something could have changed, so best reload everything
2. this isn't unique to SK - the same thing would happen with a regular form submission - you're reloading the page, so you have to reload the data (or implement some sort of caching)

but because we're not completely reloading the page, we're able to customize our app to only update the data we know have changed
-->

---

```diff
<form
	action={isFavorite ? '?/unfavorite' : '?/favorite'}
	method="POST"
	use:enhance={(event) => {
		isSubmitting = true;
		return async ({ update, result }) => {
-			await update();
+			data.isFavorite = !data.isFavorite;
			isSubmitting = false;
		};
	}}
>
```

<!--
so the solution for this page is on the simpler side - instead of calling update, which performs SK's default data refresh behavior, we just update the data ourselves

and there is some nuance here, but the example I want to focus on is actually the favorites page because that setup is more interesting. let's do some live coding
-->

---
layout: fact
---

# time for a bit of live coding 🫣

<!--
- demonstrate problem - you can't update the state of the layout
- use store instead
- then you can update the store
- handle redirect
- invalidate app:favorites

so that's really bringing it all together - we're using a form so we make for a more resilient app, but also customizing that to load the minimum data necessary when the data changes
-->

---
layout: fact
---

# how does SvelteKit help build _efficient_ and _resilient_ apps

<style>
  h1 {
    line-height: 1.2 !important;
  }
</style>

---
layout: two-cols
---

# efficiency

<v-clicks>

- fetch before rendering 
- don't overfetch (use layout loads for shared data)
- fetch in parallel
- prefetch when you can
- avoid re-fetching
- ship less JS

</v-clicks>

::right::

# resiliency

<v-clicks>

- JS is an enhancement
- building on the web platform (links & forms)
  - but enhanced!

</v-clicks>

<style>
  ul {
    padding-right: 0.5rem;
  }
</style>

<!--
recap
-->

---

# Where to next?

- the tutorial: learn.svelte.dev
- the docs: svelte.dev / kit.svelte.dev
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

---
layout: image
image: "/that/Save_the_Date.png"
---

---
layout: image
image: "/that/Sponsors.png"
---

---
layout: image
image: "/that/Session_Survey_Speaker.png"
---

<img src="/that/QR.png" class="qr">

<style>
  .qr {
    max-width: 300px;
    margin: 3rem auto 0 auto;
  }
</style>

<!--
Note that talk title is old talk title
-->
