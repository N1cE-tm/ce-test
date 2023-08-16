<svelte:options
	customElement={{
		// tag: "dpg-aldar-ui-header",
		tag: "",
		props: {
			name: { reflect: true, type: "Number", attribute: "element-index" },
		},
	}}
/>

<script lang="ts">
	import { get_current_component, onMount, setContext, createEventDispatcher } from "svelte/internal";
	import Header from "./Header.svelte";

	/**
	 * Props
	 */
	export let logo = "Logo";
	export let isCollapsed = false;
	export let hasCollapser = true;
	export let breadcrumbs = [];
	export let active = "";
	export let delimetr = `<svg width="7" height="13" viewBox="0 0 7 13" xmlns="http://www.w3.org/2000/svg"><path d="M0.13623 1.5146L0.13623 11.7627C0.13623 12.2866 0.760591 12.5589 1.1445 12.2024L6.66273 7.07835C6.91837 6.84097 6.91837 6.43638 6.66273 6.199L1.1445 1.07492C0.760592 0.718436 0.13623 0.990702 0.13623 1.5146Z"/></svg>`;

	/**
	 * Hacks
	 */
	const component = get_current_component();

	// console.log(component);

	// const emit = (name, detail) => component.dispatchEvent(new CustomEvent(name, { detail }));
	const emit = createEventDispatcher();
	let mounted = false;
	onMount(() => {
		mounted = true;

		return () => (mounted = false);
	});

	setContext("emit", emit);
</script>

{#if mounted}
	<Header {logo} {isCollapsed} {hasCollapser} {breadcrumbs} {active} {delimetr} />
{/if}
<slot />

<style lang="scss">
	:host {
		max-width: 100%;
	}
</style>
