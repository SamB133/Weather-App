<script>
	import { OPENWEATHERMAP_API_KEY } from "./env.json";

	let location;
	let showData = false;
	let mainDescription,
		description,
		icon,
		temp,
		feelsLike,
		tempMin,
		tempMax,
		humidity,
		windSpeed,
		windDegrees,
		windGust,
		name,
		code,
		message;

	function searchLocation(location) {
		let search;
		if (location < 100000 && location.toString().length === 5) {
			search = `zip=${location}`;
		} else {
			search = `q=${location}`;
		}

		fetch(
			`http://api.openweathermap.org/data/2.5/weather?${search}&units=imperial&appid=${OPENWEATHERMAP_API_KEY}`
		)
			.then((response) => {
				return response.json();
			})
			.then((data) => {
				if (data.cod === 200) {
					code = data.cod;
					mainDescription = data.weather[0].main;
					description = data.weather[0].description;
					icon = data.weather[0].icon;
					temp = data.main.temp;
					feelsLike = data.main.feels_like;
					tempMin = data.main.temp_min;
					tempMax = data.main.temp_max;
					humidity = data.main.humidity;
					windSpeed = data.wind.speed;
					windDegrees = data.wind.deg;
					windGust = data.wind.gust;
					name = data.name;
				} else {
					code = data.cod;
					message = data.message;
				}
			});

		showData = true;
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

	{#if showData}
		{#if code === 200}
			<p>
				<img
					alt="Weather Icon"
					src="http://openweathermap.org/img/wn/{icon}@2x.png"
				/>
			</p>
			<p>
				The weather in {name} is {mainDescription} with {description}.
			</p>
			<p>
				The temperature is {temp}°F and feels like {feelsLike}°F, with a
				high of {tempMax}°F and a low of {tempMin}°F.
			</p>
			<p>
				The humidity level is {humidity}%.
				{#if windSpeed > 0}
					{#if windGust === undefined}
						Wind speed is {windSpeed}MPH heading at {windDegrees}°.
					{:else}
						Wind speed is {windSpeed}MPH heading at {windDegrees}°,
						with gusts at {windGust}MPH.
					{/if}
				{/if}
			</p>
		{:else}
			<p>Error {code}: {message}</p>
		{/if}
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
