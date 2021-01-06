<template>
	<div class="home">
		<div class="container">
			<div>
				<button class="degreeButton" @click="getCel">
					°C
				</button>
				<h3 class="degree">
					0°C
				</h3>
				<button class="degreeButton" @click="getFahr">
					°F
				</button>
			</div>
			<div>
				Longitude
				<input class="field" placeholder="Longitide" v-model="longitude"/>
				Latitude
				<input class="field" placeholder="Latitude" v-model="latitude"/>
				Country/city
				<input placeholder="City" v-model="city"/>
				<button>
					Favor
				</button>
			</div>
			<div>
				<button>
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
		longitude: "",
		latitude: "",
		city: ""
	}),
	computed:
	{},
	methods:
	{
		getCel: function ()
		{
			fetch()
			.then((response) =>
			{
				return response.json();
			})
			.then((data) =>
			{
				console.log("Success", data);
			})
			.catch((error) =>
			{
				console.log("Error:", error)
			});
		},
		getFahr: function ()
		{}
	}
}
</script>

<style scoped src="./Home.css"></style>
