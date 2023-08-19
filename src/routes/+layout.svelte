<script>
	import '@fontsource/poppins/400.css';
	import '@fontsource/poppins/700.css';
	import '@fontsource/poppins/800.css';
</script>

<svelte:head>
	<title>Age Calculator | okreate</title>
</svelte:head>

<main>
	<slot />
	<footer>
		<p>
			<a
				href="https://www.frontendmentor.io/challenges/age-calculator-app-dF9DFFpj-Q"
				target="_blank">Frontend Mentor Age Calculator App</a
			>
			solution by <a href="okreate.me" target="_blank">okreate</a>
		</p>
	</footer>
</main>

<style>
	main {
		position: relative;
		height: 100%;
		background-color: hsl(0, 0%, 94%);
		display: flex;
		align-items: center;
		justify-content: center;
		padding-inline: 1rem;
	}

	footer {
		position: absolute;
		bottom: 1rem;
		text-align: center;
		padding-inline: 1rem;
	}

	a {
		color: hsl(259, 100%, 65%);
	}
</style>
