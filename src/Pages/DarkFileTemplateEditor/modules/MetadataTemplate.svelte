<script>
	import { createEventDispatcher } from "svelte"
	import NextPage from "../svg/nextPage.svelte"
	import DisabledPrevButton from "../svg/disabledPrevButton.svelte"
	import DropdownArrow from "../../FileTemplateEditor/svgIcons/DropdownArrow.svelte"

	const dispatch = createEventDispatcher()

	let pagination = true
	let active = true
	let nameFills = ["Quotaions", "Balance", "Delivery Note", "purchase Order", "E-Invoice", "Payment Receipt"]
	let activeNameFill = "Select Template"
	let KeyValues = false
	let container
	const onWindowClick = (e) => {
		if (container.contains(e.target) == false) KeyValues = false
	}
</script>

<svelte:window on:click={onWindowClick} />
<div bind:this={container}>
	<h1 class="text-primary_blue mb-4 pt-8 pl-8 text-4xl dark:text-blue-400">
		MetaData
		<br />
		Template
	</h1>
	<div class="mt-4 grid grid-cols-2 items-center 2xl:mt-16">
		<div class=" text-secondaryGray  dark:text-primaryGray px-8">
			<p class="text-justify text-sm">Select the <strong>MetaData Template </strong> from which key values will be imported for autocompleting Name fills</p>
		</div>
		<div class="relative mx-auto w-2/3">
			<button on:click={() => (KeyValues = !KeyValues)} type="button" class="text-topNavSvgBg focus:ring-primary_blue dark:bg-secondary dark:text-primaryGray group relative w-full rounded-md border border-gray-300 py-2 px-4 text-left text-sm uppercase focus:outline-none focus:ring-2 dark:border-none">
				{activeNameFill}
				<span class="absolute top-0 right-1.5 flex h-full items-center">
					<!-- <KeyValueArrow /> -->
					<DropdownArrow />
				</span>
			</button>
			<div class:hidden={!KeyValues} class="bg-heading dark:bg-secondary dark:ring-primary_blue absolute z-20 mt-2 w-full rounded-md text-sm ring-1">
				{#each nameFills as nameFill}
					<option
						on:click={() => {
							activeNameFill = nameFill
							KeyValues = false
						}}
						class="SelectDocumentType hover:bg-gray-300 dark:text-[#949494] dark:hover:bg-[#353535] dark:hover:text-[#949494]"
					>
						{nameFill}
					</option>
				{/each}
			</div>
		</div>
	</div>

	<div class:hidden={!pagination} class="absolute inset-x-0 bottom-8 flex items-center ">
		<div class="mx-auto flex">
			<nav class="flex flex-row items-center gap-4 md:justify-center">
				<button class="cursor-not-allowed " title="prev page">
					<DisabledPrevButton />
				</button>
				<button
					class="pagination"
					class:active
					title="Page 1"
					on:click={() => {
						dispatch("Component", 0)
					}}
				/>
				<button
					class="pagination"
					title="Page 2"
					on:click={() => {
						dispatch("Component", 1)
					}}
				/>
				<button
					class="pagination"
					title="Page 3"
					on:click={() => {
						dispatch("Component", 2)
					}}
				/>
				<button
					class="text-base"
					title="next page"
					on:click={() => {
						dispatch("Component", 1)
					}}
				>
					<NextPage />
				</button>
			</nav>
		</div>
	</div>
</div>

<style lang="postcss">
	.SelectDocumentType {
		@apply text-topNavSvgBg w-full cursor-pointer rounded-md py-2 px-4 text-sm;
	}
	.active {
		@apply bg-primary_blue;
	}
</style>
