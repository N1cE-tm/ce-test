<script lang="ts">
	import Path from "./Path.svelte";

	/**
	 * Props
	 */
	export let isCollapsed = false;
	export let breadcrumbs = [];
	export let active = "";
	export let delimetr;

	$: list = breadcrumbs.filter((i) => (!!isCollapsed && i === active) || !isCollapsed);

	const shadows = (node: HTMLElement, scroller: string) => {
		const items = node.querySelector<HTMLElement>(scroller);

		const cb = () => {
			let cssText =
				`--t: ${items.scrollTop}px;` +
				`--b: ${items.scrollHeight - items.offsetHeight - items.scrollTop}px;` +
				`--l: ${items.scrollLeft}px;` +
				`--r: ${items.scrollWidth - items.offsetWidth - items.scrollLeft}px;`;

			requestAnimationFrame(() => (node.style.cssText = cssText));
		};

		const observer = new ResizeObserver(cb);
		observer.observe(items);
		items.addEventListener("scroll", cb);

		return {
			update: () => {},
			destroy: () => {
				observer.disconnect();
				items.removeEventListener("scroll", cb);
			},
		};
	};
</script>

<div class="breadcrumbs" use:shadows={".items"}>
	<div class="items">
		{#each list as item (item)}
			<Path name={item} isActive={item === active} {delimetr} />
		{/each}
	</div>
	<slot />
</div>

<style lang="scss" global>
	.breadcrumbs {
		padding: 12px 0;
		flex: 1;
		position: relative;
		display: flex;
		align-items: center;
		flex-wrap: nowrap;
		overflow: hidden;
		width: 100%;

		.items {
			display: flex;
			align-items: center;
			flex-wrap: nowrap;
			overflow: auto;
			padding: 12px 10px 12px 20px;

			scrollbar-width: thin;
			scrollbar-color: #525252 transparent;
			-webkit-overflow-scrolling: touch;

			&::-webkit-scrollbar {
				appearance: none;
				-moz-appearance: none;
				-webkit-appearance: none;
				background: transparent;
				width: 3px;
				height: 3px;
			}

			&::-webkit-scrollbar-thumb {
				border-radius: 4px;
				background: #525252;
				-webkit-box-shadow: 0 0 1px rgba(255, 255, 255, 0.5);
				-moz-box-shadow: 0 0 1px rgba(255, 255, 255, 0.5);
				box-shadow: 0 0 1px rgba(255, 255, 255, 0.5);
			}
		}

		&::before {
			content: "";
			position: absolute;
			top: 0;
			bottom: 0;
			left: 0;
			right: 0;
			pointer-events: none;
			--m: var(--scroll-shadow-size, 14) * 1px;
			background: var(--scroll-shadow-top, radial-gradient(farthest-side at 50% 0%, #0003, #0000)) top / 100%
					min(var(--t), var(--m)),
				var(--scroll-shadow-bottom, radial-gradient(farthest-side at 50% 100%, #0003, #0000)) bottom / 100%
					min(var(--b), var(--m)),
				var(--scroll-shadow-left, radial-gradient(farthest-side at 0%, #0003, #0000)) left /
					min(var(--l), var(--m)) 100%,
				var(--scroll-shadow-right, radial-gradient(farthest-side at 100%, #0003, #0000)) right /
					min(var(--r), var(--m)) 100%;
			background-repeat: no-repeat;
		}
	}
</style>
