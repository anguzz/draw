<script>
    import Canvas from '../components/Canvas.svelte';
    import Palette from '../components/Colors.svelte';

	const colors = [
        '#ff0000',
        '#006600',
        '#ffff00',
        '#003d99',
        '#000000',

	]
	const background = '#fff'

	let color = colors[0]
	const paletteColor = color
</script>

<main>
	<Canvas 
					{color} 
					{background} />
	<Palette 
					 {paletteColor}
					 {colors}
					 {background}
					 on:color="{({ detail }) => {
							color = detail.color
						}}"/>
</main>

<style>	


	:global(html) {
		margin: 0;
		padding: 0;
		overflow: hidden; /* Prevent scrolling */
	}
	:global(body) {
		margin: 0;
		padding: 0;
		min-height: 100vh;
		display: flex;
		justify-content: center;
		align-items: center;
		background: #375082;
        overflow: hidden; /* Prevent scrolling, bad thing is if resize too large hard to see other buttons */

	}
	
	:global(.visually-hidden:not(:focus):not(:active)) {
		clip: rect(0 0 0 0);
		clip-path: inset(50%);
		height: 1px;
		width: 1px;
		overflow: hidden;
		position: absolute;
		white-space: nowrap;
	}
	
	main {
		max-width: 300px;
		display: flex;
		flex-direction: column;
		gap: 0.5rem 0;
	}
	
	main :global(canvas) {
		align-self: center;
	}
	
</style>