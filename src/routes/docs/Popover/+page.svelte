<script lang="ts">
	import { Popover } from "$lib";

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
