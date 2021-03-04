# svrollbar

svrollbar is the custom scrollbar made by svelte.

## how to install

```bash
npm install svrollbar
```

## examples

example svelte REPL is [here](https://svelte.dev/repl/d600db3bde4742ec8d9751e009d94159?version=3.35.0).

## how to use

svrollbar has two components; `Svrollbar.svelte` and `Svroller.svelte`.  
if you would like to make things simpler, you may prefer `Svroller.svelte`.

```svelte
<script lang="ts">
  import { Svroller } from 'svrollbar'

  const items = Array.from({ length: 50 }).map((_, i) => `item ${i}`)
</script>

<Svroller width="10rem" height="10rem">
  {#each items as item (item)}
    <div>{item}</div>
  {/each}
</Svroller>
```

on the other hand, it is better to use `Svrollbar.svelte`
if you already have a kind of scrollable viewport or contents.

```svelte
<script lang="ts">
  import { Svrollbar } from 'svrollbar'

  const items = Array.from({ length: 50 }).map((_, i) => `item ${i}`)

  export let viewport: HTMLElement
  export let contents: HTMLElement
</script>

<style>
  .wrapper {
    position: relative;
    width: 10rem;
  }

  .viewport {
    position: relative;
    width: 10rem;
    height: 10rem;
    overflow: scroll;
    border: 1px solid gray;
    box-sizing: border-box;

    /* hide scrollbar */
    -ms-overflow-style: none;
    scrollbar-width: none;
  }

  .viewport::-webkit-scrollbar {
    /* hide scrollbar */
    display: none;
  }
</style>

<div class="wrapper">
  <div bind:this={viewport} class="viewport">
    <div bind:this={contents} class="contents">
      {#each items as item (item)}
        <div>{item}</div>
      {/each}
    </div>
  </div>
  <Svrollbar {viewport} {contents} />
</div>
```

## integration

if you would like to integrate svrollbar into some kind of virtual list
implemenation such as
[svelte-virtual-list](https://github.com/sveltejs/svelte-virtual-list)
or
[svelte-tiny-virtual-list](https://github.com/Skayo/svelte-tiny-virtual-list),
you can do that in the following way.

```svelte
<script lang="ts">
  import { onMount } from 'svelte'
  import VirtualList from 'svelte-tiny-virtual-list'
  import { Svrollbar } from 'svrollbar'

  const items = Array.from({ length: 50 }).map((_, i) => `item ${i}`)

  let viewport: HTMLElement
  let contents: HTMLElement

  onMount(() => {
    viewport = document.querySelector('.virtual-list-wrapper')
    contents = document.querySelector('.virtual-list-inner')
  })
</script>

<style>
  :global(.virtual-list-wrapper) {
    /* hide scrollbar */
    -ms-overflow-style: none !important;
    scrollbar-width: none !important;
  }

  :global(.virtual-list-wrapper::-webkit-scrollbar) {
    /* hide scrollbar */
    display: none !important;
  }

  .wrapper {
    position: relative;
    width: 10rem;
  }
</style>

<div class="wrapper">
  <Svrollbar {viewport} {contents} />
  <VirtualList width="10rem" height={160} itemCount={items.length} itemSize={16}>
    <div slot="item" let:index let:style {style}>
      {items[index]}
    </div>
  </VirtualList>
</div>
```

## how to customize

you can customize svrollbar style with css variables.

| variable                  | default |
| ------------------------- | ------- |
| --svrollbar-track-width   | 10px    |
| --svrollbar-track-color   | initial |
| --svrollbar-track-radius  | initial |
| --svrollbar-thumb-opacity | 0       |
| --svrollbar-thumb-width   | 6px     |
| --svrollbar-thumb-color   | #454545 |
| --svrollbar-thumb-radius  | 0.25rem |
| --svrollbar-thumb-opacity | 0.5     |

```svelte
<script lang="ts">
  import { Svroller } from 'svrollbar'

  const items = Array.from({ length: 50 }).map((_, i) => `item ${i}`)
</script>

<style>
  .container {
    border: 3px solid #5d7150;
    width: 10rem;

    --svrollbar-track-width: 20px;
    --svrollbar-track-color: #85b4b9;
    --svrollbar-thumb-opacity: 0;

    --svrollbar-thumb-width: 10px;
    --svrollbar-thumb-color: #d9ab55;
    --svrollbar-thumb-opacity: 1;
  }
</style>

<div class="container">
  <Svroller width="10rem" height="10rem">
    {#each items as item (item)}
      <div>{item}</div>
    {/each}
  </Svroller>
</div>
```

## todos

- [ ] always visible scrollbar
- [x] scrollbar fade timeout
- [ ] horizontal scroll support
- [ ] drop shadow support
- [x] draggable thumb to scroll