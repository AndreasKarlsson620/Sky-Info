<template>
	<div class="home">
		<div class="content">
			<div>
				Ikon
			</div>
			<div class="pickDigree">
				<button class="celButton" :disabled="buttonDisabled" @click="getCel">
					°C
				</button>
				<h3 class="degree">
					0°C
				</h3>
				<button class="fahrButton" :disabled="buttonDisabled" @click="getFahr">
					°F
				</button>
			</div>
			<div class="latFavLong">
				<input class="latField" type="number" placeholder="Latitude" maxlength="12" v-model="latitude" :disabled="cordDisabled" :required="cordRequired"/>
				<button class="favButton">
					Favorite
				</button>
				<input class="longField" type="number" placeholder="Longitide" maxlength="13" v-model="longitude" :disabled="cordDisabled" :required="cordRequired"/>
			</div>
			<div class="cityDiv">
				<input class="cityField" placeholder="City name" v-model="city_name" :disabled="cityDisabled" :required="cityRequired"/>
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
		latitude: "",
		longitude: "",
		city_name: ""
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
			})
			.catch((error) =>
			{
				console.log(`Error: ${error}`)
			});
		}
	}
}
</script>

<style scoped src="./Home.css"></style>
