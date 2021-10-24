<script>
	let location;

	function searchLocation(location) {
		location;
	}
</script>

<main>
	<h1>Find local weather</h1>
	<input
		on:keypress={(e) => {
			if (e.key === "Enter") searchLocation(location);
		}}
		placeholder="Enter City or Zip"
		bind:value={location}
	/>
	<button on:click={searchLocation(location)}>Search</button>
</main>
