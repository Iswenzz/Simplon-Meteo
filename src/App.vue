<template>
	<div id="app">
		<div class="page">
			<h1>Simplon Météo</h1>
			<input class="searchbar" type="text" v-model="city">
			<div v-if="this.data" class="card">
				<div>
					<img :src="`http://openweathermap.org/img/wn/${this.data.weather[0].icon}@4x.png`"
						:alt="this.data.weather[0].description">
				</div>
				<div>
					<h3 class="card-main">{{ this.data.name }}</h3>
					<h2 class="card-main">{{ this.data.weather[0].main }}</h2>
					<h3 class="card-degree">{{ this.data.main.temp + "°C" }}</h3>
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
html, body, #app {
	margin: 0;
	padding: 0;
	background: linear-gradient(90deg, #FFE53B 0%, #fd3838 74%);
	font-family: Avenir, Helvetica, Arial, sans-serif;
	color: #fff;
	text-align: center;
}

.card {
	display: flex;
	justify-content: center;
	align-items: center;
	flex-direction: column;
	width: 500px;
	height: 400px;
	max-width: 800px;
	margin: auto;
	background: #fff;
	box-shadow: 0 14px 80px rgba(34, 35, 58, 0.2);
	border-radius: 30px;
	color: black;
}

.searchbar {
	width: 440px;
	height: 30px;
	margin: 1em;
	border: none;
	border-radius: 30px;
	color: #fff;
	background: dimgray;
	padding: 0 30px;
}
</style>
