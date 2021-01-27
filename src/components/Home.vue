<template>
	<div class="home">
		<div class="content">
			<div>
				<img :src="icon" alt="Weather icon"/>
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
				<div class="feeMinMax">
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
				</div>
				<div class="presHu">
					<div class="presDiv">
						<h4 class="pressure">
							Pressure
						</h4>
						{{pressure}}
					</div>
					<div class="humiDiv">
						<h4 class="humidity">Humidity</h4>
						{{humidity}}
					</div>
				</div>
			</div>
			<div class="latFavLong">
				<input class="latField" type="number" placeholder="Latitude"
				maxlength="12" v-model="latitude" :disabled="cordDisabled"
				:required="cordRequired"/>
				<button class="favButton">
					Favorite
				</button>
				<input class="longField" type="number" placeholder="Longitide"
				maxlength="13" v-model="longitude" :disabled="cordDisabled"
				:required="cordRequired"/>
			</div>
			<div class="cityDiv">
				<input class="cityField" placeholder="City name"
				v-model="city_name" :disabled="cityDisabled"
				:required="cityRequired"/>
			</div>
			<div class="historyDiv">
				<button class="historyButton">
					HISTORY
				</button>
			</div>
		</div>
	</div>
</template>

<script>
export default
{
	name: 'Home',
	components:
	{},
	props:
	{},
	data: () =>
	({
		api_key: "049e1941f5b20f4a522aafc37bf32823",
		url_base: "https://api.openweathermap.org/data/2.5/weather",
		weather: "",
		description: "",
		temp: "0",
		feels_like: "",
		temp_min: "",
		temp_max: "",
		pressure: "",
		humidity: "",
		latitude: "",
		longitude: "",
		city_name: "",
		icon: ""
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
	methods:
	{
		getCel: function ()
		{
			fetch(`${this.url_base}?lat=${this.latitude}&lon=${this.longitude}&units=metric&appid=${this.api_key}`)
			.then((response) =>
			{
				return response.json();
			})
			.then((data) =>
			{
				console.log(`Success: ${JSON.stringify(data)}`);
				this.weather = data.weather[0].main;
				let description = data.weather[0].description;
				this.description = `${description[0].toUpperCase()}${description.slice(1)}`;
				this.temp = `${data.main.temp} °C`;
				this.feels_like = `${data.main.feels_like} °C`;
				this.temp_min = `${data.main.temp_min} °C`;
				this.temp_max = `${data.main.temp_max} °C`;
				this.pressure = `${data.main.pressure} hPa`;
				this.humidity = `${data.main.humidity} %`;
				this.icon = `http://openweathermap.org/img/wn/${data.weather[0].icon}@2x.png`;
			})
			.catch((error) =>
			{
				console.log(`Error: ${error}`)
			});
		},
		getFahr: function ()
		{
			fetch(`${this.url_base}?lat=${this.latitude}&lon=${this.longitude}&units=imperial&appid=${this.api_key}`)
			.then((response) =>
			{
				return response.json();
			})
			.then((data) =>
			{
				console.log(`Success: ${JSON.stringify(data)}`);
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
		},
		saveFav: function ()
		{
			if(this.latitude && this.longitude)
			{
				localStorage.setItem('favCords', this.latitude, this.longitude);
			}
			else
			{
				localStorage.setItem('favCity', this.city_name);
			}
		}
	}
}
</script>

<style scoped src="./Home.css"></style>
