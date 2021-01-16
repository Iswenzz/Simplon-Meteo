<template>
	<div id="app">
		<div class="page">
			<h1 class="header">Simplon Météo</h1>
			<input class="searchbar" type="text" v-model="city">
			<div v-if="this.data" class="card">
				<div class="card-icon">
					<h4>{{ "Coords " + this.data.coord.lat + ", " + this.data.coord.lon }}</h4>
					<img :src="`http://openweathermap.org/img/wn/${this.data.weather[0].icon}@4x.png`"
						:alt="this.data.weather[0].description">
					<h3>{{ "Min " + this.data.main.temp_min + "°C, Max " + this.data.main.temp_max + "°C" }}</h3>
				</div>
				<div class="card-info">
					<h2>{{ this.data.name }}</h2>
					<time>{{ new Date().toLocaleString("fr-FR") }}</time>
					<h2>{{ this.data.weather[0].main }}</h2>
					<div>
						<h3>{{ this.data.main.temp + "°C" }}</h3>
						<h3>{{ "Ressentie " + this.data.main.feels_like + "°C" }}</h3>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: "App",
	data() {
		return {
			city: "Grenoble",
			data: null
		};
	},
	async created() {
		await this.fetchWeather();
	},
	methods: {
		async fetchWeather() {
			const response = await fetch(
				`http://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=${process.env.VUE_APP_WEATHER_API_KEY}`
			);
			const json = await response.json();
			console.log(json);
			this.data = json;
		}
	},
	watch: {
		async city() {
			await this.fetchWeather();
		}
	}
};
</script>

<style lang="scss">
$queries: (
	desktop: "min-width: 1224px",
	tablet_mobile: "max-device-width: 1224px",
	portrait: "orientation: portrait",
	retina: "min-resolution: 2dppx",
	big: "min-device-width: 1824px"
);

@mixin media($keys...) {
	@each $key in $keys {
		@media (map-get($queries, $key)) {
			@content;
		}
	}
}

html, body, #app {
	margin: 0;
	padding: 0;
	background-color: #1c1c1c;
	font-family: Avenir, Helvetica, Arial, sans-serif;
	color: #fff;
	text-align: center;
}

.header {
	margin-top: 0;
	padding: 0.8em;
	background-color: #0e0e0e;
}

.card {
	display: flex;
	padding-top: 3em;
	justify-content: center;
	align-items: center;
	width: 60%;
	height: 400px;
	margin: auto;
	box-shadow: 0 14px 80px rgba(34, 35, 58, 0.2);
	color: white;
	flex-direction: row;

	#{&}-info {
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
		color: black;
		background-color: white;
		width: 100%;
		height: 100%;
		border-top-right-radius: 50px;
		border-bottom-right-radius: 50px;

		@include media(tablet_mobile, portrait) {
			border-top-right-radius: 0;
			border-bottom-right-radius: 50px;
			border-bottom-left-radius: 50px;
		}
	}

	#{&}-icon {
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
		background: linear-gradient(140deg, #0d23e7 0%, #00e1ff 74%);
		width: 100%;
		height: 100%;
		border-top-left-radius: 50px;
		border-bottom-left-radius: 50px;

		@include media(tablet_mobile, portrait) {
			border-top-right-radius: 50px;
			border-top-left-radius: 50px;
			border-bottom-left-radius: 0;
		}
	}

	@include media(tablet_mobile, portrait) {
		flex-direction: column;
		width: 80%;
	}
}

.searchbar {
	width: 40%;
	height: 30px;
	border: none;
	margin: 1em;
	border-radius: 30px;
	color: #fff;
	background: dimgray;
	padding: 0 30px;

	@include media(tablet_mobile, portrait) {
		margin-bottom: 5em;
	}
}
</style>
