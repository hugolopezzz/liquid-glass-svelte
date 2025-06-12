<script lang="ts">
	import LiquidGlass from '$lib/LiquidGlass.svelte';
</script>

<div class="test">
	<img
		src="https://images.unsplash.com/photo-1734607947797-2a61b996fd5d?q=80&w=4500&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D"
		alt=""
	/>
	<LiquidGlass
		options={{
			edgeBlur: '0.1vw',
			mainBlur: '1vw'
		}}
		class="text"
	>
		<h1>liquid-glass-svelte</h1>
	</LiquidGlass>
</div>

<style>
	.test {
		height: 100vh;
		width: 100vw;
		position: relative;

		display: flex;
		align-items: center;
		justify-content: center;

		img {
			width: 100%;
			height: 100%;
			object-fit: cover;
			position: absolute;
			top: 0;
			left: 0;
			z-index: -1;
		}

		:global(.text) {
			font-family: 'Montserrat', sans-serif;
			color: white;

			padding: 3vw;
			border-radius: 2vw;
		}
	}
</style>
