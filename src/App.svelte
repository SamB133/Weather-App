<script>
	let location;
</script>

<main>
	<h1>Find local weather</h1>
	<input placeholder="Enter City or Zip" bind:value={location} />
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: blue;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 300;
	}

	input {
		text-align: center;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
