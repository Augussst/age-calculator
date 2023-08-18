<script>
	import '@fontsource/poppins/400.css';
	import '@fontsource/poppins/700.css';
	import '@fontsource/poppins/800.css';
</script>

<main>
	<slot />
</main>

<style>
	main {
		height: 100%;
		background-color: hsl(0, 0%, 94%);
		display: flex;
		align-items: center;
		justify-content: center;
		padding-inline: 1rem;
	}
</style>
