<script lang="ts">
	import "../app.postcss";

	import { dev } from "$app/environment";

	import SkipLink from "$site/components/SkipLink.svelte";
	import NavItems from "$site/components/NavItems.svelte";
	import Sheet from "$lib/components/Sheet.svelte";

	import Bars from "$site/svg/Bars.svelte";
	import X from "$site/svg/X.svelte";
	import Breakpoint from "$lib/components/Breakpoint.svelte";

	let displaySheet = false;

	const closeSheet = () => {
		displaySheet = false;
	};
</script>

{#if dev}
	<Breakpoint
		class="fixed bottom-4 right-4 rounded border bg-white px-2 py-1 font-mono tabular-nums shadow"
	/>
{/if}

<SkipLink />

<div
	class="prose-h1:text-balance prose prose-neutral max-w-none font-antique selection:bg-neutral-400/30 prose-h1:capitalize prose-h2:capitalize prose-h3:capitalize prose-a:underline-offset-2 hover:prose-a:decoration-dotted focus:prose-a:decoration-dotted prose-pre:text-sm lg:flex lg:justify-center"
>
	<div class="gap-8 lg:flex">
		<header class="sticky top-0">
			<nav class="sticky top-0 hidden lg:block">
				<ul class="my-0 h-screen list-none overflow-y-auto p-4">
					<li>
						<h2 class="mb-6 mt-0">
							<a class="font-extrabold lowercase no-underline" href="/">drab</a>
						</h2>
					</li>
					<NavItems />
				</ul>
			</nav>
			<nav
				class="mb-6 flex items-center gap-4 bg-neutral-50/75 p-4 backdrop-blur-lg lg:hidden lg:w-[80ch]"
			>
				<button
					class="btn btn-s"
					title="Components"
					on:click={() => (displaySheet = true)}
				>
					<Bars />
				</button>
				<h2 class="my-0">
					<a class="font-extrabold lowercase no-underline" href="/">drab</a>
				</h2>
			</nav>
		</header>
		<main id="main" class="m-4 mb-16 max-w-[65ch] lg:w-[65ch]">
			<slot />
			<Sheet
				bind:display={displaySheet}
				class="backdrop-blur"
				classSheet="p-4 shadow bg-white overflow-y-auto"
			>
				<div class="flex items-center justify-between gap-4">
					<h2 class="my-0">
						<a
							on:click={closeSheet}
							class="font-extrabold lowercase no-underline"
							href="/"
						>
							drab
						</a>
					</h2>
					<button title="Close" on:click={closeSheet} class="btn btn-s">
						<X />
					</button>
				</div>
				<ul class="mb-8 w-60 list-none pl-0">
					<NavItems linkClick={closeSheet} />
				</ul>
			</Sheet>
		</main>
	</div>
</div>
