<script lang="ts">
	import { slide } from "svelte/transition";
	import { cubicInOut } from "svelte/easing";
	import { getContext, createEventDispatcher } from "svelte";

	export let name;
	export let delimetr;
	export let isActive = false;

	const emit = getContext<any>("emit");
	const dispatch = createEventDispatcher();
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<div
	transition:slide|local={{ axis: "x", easing: cubicInOut }}
	on:transitionend={() => dispatch("transition-end")}
	class="path"
>
	<!-- svelte-ignore a11y-no-static-element-interactions -->
	<div class="name" on:click={() => emit("click-path", name)} class:active={isActive}>
		{name}
	</div>
	<div class="delimetr">
		{@html delimetr}
	</div>
</div>

<style lang="scss" global>
	.path {
		display: flex;
		align-items: center;
		justify-content: center;
		overflow: hidden;
		flex: none;

		.name {
			display: flex;
			padding: 4px 16px;
			background-color: #595959;
			color: #bdbdbd;
			border-radius: 8px;
			transition: 0.3s;
			margin-right: 10px;

			&.active {
				background-color: #dadada;
				color: #2f2f2f;
			}
		}

		.delimetr {
			display: flex;
			overflow: initial;
			padding-right: 10px;
			fill: #595959;
		}

		&:last-child .delimetr {
			display: none;
		}
	}
</style>
