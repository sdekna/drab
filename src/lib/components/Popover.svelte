<!--
@component

### Popover

Displays a popover in relation to the `target`. 

- Does not require the target to be `position: relative;`
- Adjusts position in case the popover renders outside of the viewport

@props

- `class` 
- `display` - shows / hides the popover
- `id` 
- `position` - where the popover is displayed in relation to the `target`
- `target` - target element to position the popover in relation to
- `transition` - scales the popover, set to `false` to disable

@slots

| name       | purpose                         | default value |
| ---------- | ------------------------------- | ------------- |
| `default`  | default                         | Popover       |

@example

```svelte
<script lang="ts">
	import { Popover } from "drab";

	let target: HTMLButtonElement;

	let display = false;

	const open = () => (display = true);
	const close = () => (display = false);
</script>

<button class="btn" type="button" bind:this={target} on:click={open}>
	Open
</button>

<Popover {target} bind:display class="p-2">
	<div class="flex w-48 flex-col gap-2 rounded border bg-white p-2 shadow">
		<div class="font-bold">Bottom</div>
		<button class="btn" on:click={close}>Close</button>
		<button class="btn" on:click={close}>Close</button>
		<button class="btn" on:click={close}>Close</button>
	</div>
</Popover>
```
-->

<script lang="ts">
	import { prefersReducedMotion } from "$lib/util/accessibility";
	import { duration, start } from "$lib/util/transition";
	import { onMount, tick } from "svelte";
	import { scale, type ScaleParams } from "svelte/transition";

	let className = "";
	export { className as class };

	export let id = "";

	/** shows / hides the popover */
	export let display = true;

	/** where the popover is displayed in relation to the `target` */
	export let position: "t" | "b" | "l" | "r" = "b";

	/** target element to position the popover in relation to */
	export let target: HTMLElement;

	/** scales the popover, set to `false` to disable */
	export let transition: ScaleParams | false = { duration, start };

	let popover: HTMLDivElement;

	let coordinates: { x: number; y: number } = { x: 0, y: 0 };

	const setPosition = async () => {
		if (position === "t" || position === "b") {
			coordinates.x = target.offsetWidth / 2 - popover.offsetWidth / 2;
			if (position === "t") {
				coordinates.y = -popover.offsetHeight;
			} else {
				coordinates.y = target.offsetHeight;
			}
		} else {
			coordinates.y = target.offsetHeight / 2 - popover.offsetHeight / 2;
			if (position === "l") {
				coordinates.x = -popover.offsetWidth;
			} else {
				coordinates.x = target.offsetWidth;
			}
		}
		const targetRect = target.getBoundingClientRect();

		coordinates.x += targetRect.x + window.scrollX;
		coordinates.y += targetRect.y + window.scrollY;

		await tick();

		const popoverRect = popover.getBoundingClientRect();

		// correct position if not visible
		if (popoverRect.x < 0) {
			coordinates.x += Math.abs(popoverRect.x);
		} else if (popoverRect.x + popover.offsetWidth > window.innerWidth) {
			coordinates.x -=
				popoverRect.x + popover.offsetWidth - window.innerWidth + 16;
		}
		if (popoverRect.y < 0) {
			coordinates.y += Math.abs(popoverRect.y);
		} else if (popoverRect.y + popover.offsetHeight > window.innerHeight) {
			coordinates.y -=
				popoverRect.y + popover.offsetHeight - window.innerHeight;
		}
	};

	const onKeyDown = (e: KeyboardEvent) => {
		if (e.key === "Escape") {
			display = false;
		}
	};

	$: if (target && popover) setPosition();

	onMount(() => {
		if (prefersReducedMotion()) transition = false;
	});
</script>

<svelte:body on:keydown={onKeyDown} />

{#if display}
	<div
		role="dialog"
		bind:this={popover}
		class={className}
		{id}
		style:top="{coordinates.y}px"
		style:left="{coordinates.x}px"
		transition:scale={transition ? transition : { duration: 0 }}
	>
		<slot>Popover</slot>
	</div>
{/if}

<style>
	div {
		position: absolute;
	}
</style>
