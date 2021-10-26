<script>
	import { OPENWEATHERMAP_API_KEY } from "./env.json";

	let location;
	let searchPromise;

	function searchLocation(location) {
		let search;
		if (location < 100000 && location.toString().length === 5) {
			search = `zip=${location}`;
		} else {
			search = `q=${location}`;
		}

		searchPromise = fetch(
			`http://api.openweathermap.org/data/2.5/weather?${search}&units=imperial&appid=${OPENWEATHERMAP_API_KEY}`
		)
			.then((response) => {
				return response.json();
			})
			.then((data) => {
				if (data.cod != 200) {
					throw new Error(data.message);
				}

				return {
					code: data.cod,
					message: data.message,
					mainDescription: data.weather[0].main,
					description: data.weather[0].description,
					icon: data.weather[0].icon,
					temp: data.main.temp,
					feelsLike: data.main.feels_like,
					tempMin: data.main.temp_min,
					tempMax: data.main.temp_max,
					humidity: data.main.humidity,
					windSpeed: data.wind.speed,
					windDegrees: data.wind.deg,
					windGust: data.wind.gust,
					name: data.name,
				};
			});
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

	{#if searchPromise}
		{#await searchPromise}
			<p>Loading, please wait...</p>
		{:then data}
			{#if data.code === 200}
				<p>
					<img
						alt="Weather Icon"
						src="http://openweathermap.org/img/wn/{data.icon}@2x.png"
					/>
				</p>
				<p>
					The weather in {data.name} is {data.mainDescription} with {data.description}.
				</p>
				<p>
					The temperature is {data.temp}°F and feels like {data.feelsLike}°F,
					with a high of {data.tempMax}°F and a low of {data.tempMin}°F.
				</p>
				<p>
					The humidity level is {data.humidity}%.
					{#if data.windSpeed > 0}
						{#if data.windGust === undefined}
							Wind speed is {data.windSpeed}MPH heading at {data.windDegrees}°.
						{:else}
							Wind speed is {data.windSpeed}MPH heading at {data.windDegrees}°,
							with gusts at {data.windGust}MPH.
						{/if}
					{/if}
				</p>
			{/if}
		{:catch err}
			<p>{err}</p>
		{/await}
	{/if}
</main>

<style>
	button:hover {
		color: darkgrey;
		cursor: pointer;
	}

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
