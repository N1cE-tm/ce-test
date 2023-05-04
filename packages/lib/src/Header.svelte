<script lang="ts">
	import Breadcrumbs from "./Breadcrumbs.svelte";

	/**
	 * Props
	 */
	export let logo = "Logo";
	export let isCollapsed = false;
	export let hasCollapser = true;
	export let breadcrumbs = [];
	export let active = "";
	export let delimetr;
</script>

<div class="header">
	<div class="logo">{logo}</div>
	<Breadcrumbs {breadcrumbs} {isCollapsed} {active} {delimetr} />
	{#if breadcrumbs?.length > 1 && hasCollapser}
		<!-- svelte-ignore a11y-click-events-have-key-events -->
		<div class="collapser" on:click={() => (isCollapsed = !isCollapsed)}>123</div>
	{/if}
</div>

<slot />

<style lang="scss" global>
	.header {
		height: 60px;
		background: rgba(35, 35, 35, 0.85);
		border-radius: 8px;
		display: flex;
		align-items: center;
		overflow: hidden;
		// gap: 20px;

		.logo {
			padding: 12px 0 12px 20px;
			flex: none;
		}

		.collapser {
			display: flex;
			align-items: center;
			justify-content: center;
			height: 100%;
			width: 25px;
			background-color: #595959;
			border-radius: 0 8px 8px 0;
			flex: none;
		}
	}
</style>
