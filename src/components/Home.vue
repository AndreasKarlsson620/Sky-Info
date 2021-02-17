<template>
	<div class="home">
		<div class="content">
			<div class="iconWeathDesc">
				<img class="icon" :src="icon" alt="Weather icon" v-if="showIcon"/>
				<h4 class="weather">
					{{weather}}
				</h4>
				<h4 class="description">
					{{description}}
				</h4>
			</div>
			<div class="pickDigree">
				<button class="celButton" :disabled="buttonDisabled"
				@click="getCel">
					°C
				</button>
				<h3 class="degree">
					{{temp}}
				</h3>
				<button class="fahrButton" :disabled="buttonDisabled"
				@click="getFahr">
					°F
				</button>
			</div>
			<div class="infoDiv">
				<div class="info" v-show="showInfo">
					<div class="feelsDiv">
						<h4 class="feels">
							Feels like
						</h4>
						{{feels_like}}
					</div>
					<div class="minDiv">
						<h4 class="min">
							Min
						</h4>
						{{temp_min}}
					</div>
					<div class="maxDiv">
						<h4 class="max">
							Max
						</h4>
						{{temp_max}}
					</div>
					<div class="presDiv">
						<h4 class="pressure">
							Pressure
						</h4>
						{{pressure}}
					</div>
					<div class="humiDiv">
						<h4 class="humidity">
							Humidity
						</h4>
						{{humidity}}
					</div>
				</div>
			</div>
			<div class="latFavLong">
				<div class="latDiv">
					<h4 class="latitude">
						Latitude
					</h4>
					<input class="latField" type="number" placeholder="Latitude" min="-90" max="90" step=".000001" v-model="latitude" :disabled="cordDisabled" :required="cordRequired"/>
				</div>
				<button class="favButton" :disabled="buttonDisabled" @click="saveFav">
					FAVORITE
				</button>
				<div class="longDiv">
					<h4 class="longitude">
						Longitude
					</h4>
					<input class="longField" type="number" placeholder="Longitide" min="-180" max="180" step=".000001" v-model="longitude" :disabled="cordDisabled" :required="cordRequired"/>
				</div>
			</div>
			<div class="cityDiv">
				<h4 class="cityName">
					City name
				</h4>
				<input class="cityField" type="text" placeholder="City name"
				v-model="city_name" :disabled="cityDisabled"
				:required="cityRequired"/>
			</div>
			<div class="favoritesDiv">
				<button class="favoritesButton" @click="getFavorites"
				:disabled="favoritesDisabled = true">
					FAVORITES
				</button>
			</div>
			<div class="historyDiv">
				<button class="historyButton" @click="getHistory">
					HISTORY
				</button>
			</div>
			<div class="historyInfo" v-if="showHistory">
				<div class="card" v-for="day in days" :key="day.date">
					<div class="hisTimeIconDiv">
						<h4 class="hisDate">
							{{day.date}}
						</h4>
						<img class="hisIcon" :src="day.icon" alt="Weather icon"/>
					</div>
					<div class="stats">
						<div class="hisWeathDesc">
							<h4 class="hisWeather">
								{{day.weather}}
							</h4>
							<h4 class="hisDescription">
								{{day.description}}
							</h4>
						</div>
						<div class="hisTempCelDiv">
							<h4 class="hisTempCel">
								Temp °C
							</h4>
							{{day.temp_cel}}
						</div>
						<div class="hisFeelsCelDiv">
							<h4 class="hisFeelsCel">
								Feels like °C
							</h4>
							{{day.feels_cel}}
						</div>
						<div class="hisTempFahrDiv">
							<h4 class="hisTempFahr">
								Temp °F
							</h4>
							{{day.temp_fahr}}
						</div>
						<div class="hisFeelsFahrDiv">
							<h4 class="hisFeelsFahr">
								Feels like °F
							</h4>
							{{day.feels_fahr}}
						</div>
					</div>
					<div class="hisPresDiv">
						<h4 class="hisPressure">
							Pressure hPa
						</h4>
						{{day.pressure}}
					</div>
					<div class="hisHumiDiv">
						<h4 class="hisHumidity">
							Humidity %
						</h4>
						{{day.humidity}}
					</div>
				</div>
			</div>
			<Geolocation/>
		</div>
	</div>
</template>

<script>
import Geolocation from './Geolocation.vue'

export default
{
	name: 'Home',
	components:
	{
		Geolocation
	},
	props:
	{},
	data: () =>
	({
		api_key: "049e1941f5b20f4a522aafc37bf32823",
		url_base: "https://api.openweathermap.org/data/2.5/weather",
		history_url: "http://api.openweathermap.org/data/2.5/onecall/timemachine",
		//Current weather
		icon: "", weather: "", description: "", temp: "0", feels_like: "",
		temp_min: "", temp_max: "", pressure: "", humidity: "", showIcon: false,
		showInfo: false,
		//Input
		latitude: "", longitude: "", city_name: "",
		//Favorite
		favId: 0, favorite: "", showFavorites: false,
		//Historical weather
		days: [],
		day: {
			icon: ""
		},
		showHistory: false
	}),
	computed:
	{
		buttonDisabled()
		{
			if(this.latitude != "" && this.longitude != "")
			{
				return false;
			}
			else if(this.city_name != "")
			{
				return false;
			}
			else
			{
				return true;
			}
		},
		cordDisabled()
		{
			if(this.city_name != "")
			{
				return true;
			}
			else
			{
				return false;
			}
		},
		cordRequired()
		{
			if(this.latitude != "" || this.longitude != "")
			{
				return true;
			}
			else
			{
				return false;
			}
		},
		cityDisabled()
		{
			if(this.latitude != "" || this.longitude != "")
			{
				return true;
			}
			else
			{
				return false;
			}
		},
		cityRequired()
		{
			if(this.city_name != "")
			{
				return true;
			}
			else
			{
				return false;
			}
		}
	},
	mounted()
	{
		if(localStorage.latitude && localStorage.longitude)
		{
			this.latitude = localStorage.latitude;
			this.longitude = localStorage.longitude;
		}
		if(localStorage.city_name)
		{
			this.city_name = localStorage.city_name;
		}
	},
	methods:
	{
		getCel: function ()
		{
			if(this.latitude && this.longitude)
			{
				fetch(`${this.url_base}?lat=${this.latitude}&lon=${this.longitude}&units=metric&appid=${this.api_key}`)
				.then((response) =>
				{
					return response.json();
				})
				.then((data) =>
				{
					console.log(`Success: ${JSON.stringify(data)}`);
					this.icon = `http://openweathermap.org/img/wn/${data.weather[0].icon}@2x.png`;
					this.weather = data.weather[0].main;
					let description = data.weather[0].description;
					this.description = `${description[0].toUpperCase()}${description.slice(1)}`;
					this.temp = `${data.main.temp} °C`;
					this.feels_like = `${data.main.feels_like} °C`;
					this.temp_min = `${data.main.temp_min} °C`;
					this.temp_max = `${data.main.temp_max} °C`;
					this.pressure = `${data.main.pressure} hPa`;
					this.humidity = `${data.main.humidity} %`;
				})
				.catch((error) =>
				{
					console.log(`Error: ${error}`)
				});
			}
			else
			{
				let cleanCityName = this.city_name.trim().replace(/[&/\\#,+()$~%.'":*?<>{}]/g, '');
				fetch(`${this.url_base}?q=${cleanCityName}&units=metric&appid=${this.api_key}`)
				.then((response) =>
				{
					return response.json();
				})
				.then((data) =>
				{
					console.log(`Success: ${JSON.stringify(data)}`);
					this.icon = `http://openweathermap.org/img/wn/${data.weather[0].icon}@2x.png`;
					this.weather = data.weather[0].main;
					let description = data.weather[0].description;
					this.description = `${description[0].toUpperCase()}${description.slice(1)}`;
					this.temp = `${data.main.temp} °C`;
					this.feels_like = `${data.main.feels_like} °C`;
					this.temp_min = `${data.main.temp_min} °C`;
					this.temp_max = `${data.main.temp_max} °C`;
					this.pressure = `${data.main.pressure} hPa`;
					this.humidity = `${data.main.humidity} %`;
				})
				.catch((error) =>
				{
					console.log(`Error: ${error}`)
				});
			}
			this.showIcon = true;
			this.showInfo = true;
		},
		getFahr: function ()
		{
			if(this.latitude && this.longitude)
			{
				fetch(`${this.url_base}?lat=${this.latitude}&lon=${this.longitude}&units=imperial&appid=${this.api_key}`)
				.then((response) =>
				{
					return response.json();
				})
				.then((data) =>
				{
					console.log(`Success: ${JSON.stringify(data)}`);
					this.icon = `http://openweathermap.org/img/wn/${data.weather[0].icon}@2x.png`;
					this.weather = data.weather[0].main;
					let description = data.weather[0].description;
					this.description = `${description[0].toUpperCase()}${description.slice(1)}`;
					this.temp = `${data.main.temp} °F`;
					this.feels_like = `${data.main.feels_like} °F`;
					this.temp_min = `${data.main.temp_min} °F`;
					this.temp_max = `${data.main.temp_max} °F`;
					this.pressure = `${data.main.pressure} hPa`;
					this.humidity = `${data.main.humidity} %`;
				})
				.catch((error) =>
				{
					console.log(`Error: ${error}`)
				});
			}
			else
			{
				let cleanCityName = this.city_name.trim().replace(/[&/\\#,+()$~%.'":*?<>{}]/g, '');
				fetch(`${this.url_base}?q=${cleanCityName}&units=imperial&appid=${this.api_key}`)
				.then((response) =>
				{
					return response.json();
				})
				.then((data) =>
				{
					console.log(`Success: ${JSON.stringify(data)}`);
					this.icon = `http://openweathermap.org/img/wn/${data.weather[0].icon}@2x.png`;
					this.weather = data.weather[0].main;
					let description = data.weather[0].description;
					this.description = `${description[0].toUpperCase()}${description.slice(1)}`;
					this.temp = `${data.main.temp} °F`;
					this.feels_like = `${data.main.feels_like} °F`;
					this.temp_min = `${data.main.temp_min} °F`;
					this.temp_max = `${data.main.temp_max} °F`;
					this.pressure = `${data.main.pressure} hPa`;
					this.humidity = `${data.main.humidity} %`;
				})
				.catch((error) =>
				{
					console.log(`Error: ${error}`)
				});
			}
			this.showIcon = true;
			this.showInfo = true;
		},
		saveFav: function ()
		{
			localStorage.clear();
			if(this.latitude != "" && this.longitude != "")
			{
				localStorage.latitude = this.latitude;
				localStorage.longitude = this.longitude;
				localStorage.favorite = `${this.latitude} ${this.longitude}`;
			}
			if(this.city_name != "")
			{
				localStorage.city_name = this.city_name;
				localStorage.favorite = `${this.city_name}`;
			}
			console.log(localStorage);
		},
		getHistory: async function ()
		{
			this.days = [];
			if (this.showHistory == false)
			{
				let latToUse;
				let lonToUse;
				if (this.latitude != "" && this.longitude != "")
				{
					latToUse = this.latitude;
					lonToUse = this.longitude;
				}
				else
				{
					latToUse = 60.99;
					lonToUse = 30.9;
				}
				for (let i = 1; i < 2; i++)
				{
					let currDay = new Date();
					console.log(`currDay: ${currDay}`);
					currDay.setDate(currDay.getDate() - i);
					console.log(`currDay2: ${currDay}`);
					let nowUtc = Date.UTC(currDay.getUTCFullYear(), currDay.getUTCMonth(),currDay.getUTCDate(),currDay.getUTCHours(),currDay.getUTCMinutes(),currDay.getUTCSeconds());
					//console.log(`nowUtc: ${nowUtc}`);
					let timeStamp = nowUtc / 1000;
					//console.log(`timeStamp: ${nowUtc/1000}`);

					await fetch(`${this.history_url}?lat=${latToUse}&lon=${lonToUse}&dt=${timeStamp}&units=metric&appid=${this.api_key}`)
					.then((response) =>
					{
						return response.json();
					})
					.then((data) =>
					{
						//console.log(`Success: ${JSON.stringify(data)}`);
						let milliseconds = data.current.dt * 1000;
						let dateObject = new Date(milliseconds);
						let humanDateFormat = dateObject.toUTCString();
						this.day.date = humanDateFormat;
						this.day.icon = `http://openweathermap.org/img/wn/${data.current.weather[0].icon}@2x.png`;
						this.day.weather = data.current.weather[0].main;
						let description = data.current.weather[0].description;
						this.day.description = `${description[0].toUpperCase()}${description.slice(1)}`;
						this.day.temp_cel = `${data.current.temp} °C`;
						this.day.feels_cel = `${data.current.feels_like} °C`;
						this.day.pressure = `${data.current.pressure} hPa`;
						this.day.humidity = `${data.current.humidity} %`;
					})
					.catch((error) =>
					{
						console.log(`Error: ${error}`);
					});
					await fetch(`${this.history_url}?lat=${latToUse}&lon=${lonToUse}&dt=${timeStamp}&units=imperial&appid=${this.api_key}`)
					.then((response) =>
					{
						return response.json();
					})
					.then((data) =>
					{
						//console.log(`Success: ${JSON.stringify(data)}`);
						this.day.temp_fahr = `${data.current.temp} °F`;
						this.day.feels_fahr = `${data.current.feels_like} °F`;
					})
					.catch((error) =>
					{
						console.log(`Error: ${error}`);
					});
					this.days.push(this.day);
				}
				console.log(`days: ${JSON.stringify(this.days)}`);
				this.showHistory = true;
			}
			else
			{
				this.showHistory = false;
			}
		},
		getFavorites: function ()
		{
			console.log(`Favorites!`);
			if (this.showFavorites == false)
			{
				this.favorite = localStorage.favorite;
				this.showFavorites = true;
			}
			else
			{
				this.showFavorites = false;
			}
		}
	}
}
</script>

<style scoped src="./Home.css"></style>
