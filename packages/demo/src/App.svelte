<script lang="ts">
	import "../../lib";
	// import Path from "../../lib/src/Path.svelte";

	let breadcrumbs = ["map", "something", "florplan", "vr"];
	let active = "vr";

	let newState = "state";

	const goTo = (name) => {
		console.log(name);

		if (!name) return;
		active = name;
		if (breadcrumbs.includes(name)) {
			breadcrumbs = breadcrumbs.slice(0, breadcrumbs.indexOf(name) + 1);
			return;
		} else {
			breadcrumbs = [...breadcrumbs, name];
			return;
		}
	};

	const addState = () => {
		goTo(newState);

		newState = "";
	};
</script>

<div class="example">
	<input type="text" bind:value={newState} />
	<button on:click={addState}>add</button>
</div>

<div class="example t">
	<n1ce-test {breadcrumbs} {active} on:click-path={(e) => goTo(e?.detail)} />
</div>

<style lang="scss">
	.example {
		width: 100%;
		display: flex;

		&.t {
			width: 250px;
		}
	}
</style>
