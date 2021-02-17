<template>
	<div class="geoLocDiv">
		<div class="geoQuesDiv">
			<p class="geoQues">
				Do you want to enable geolocation to see information about the current location?
			</p>
		</div>
		<div class="geoButtDiv">
			<button class="geoButton" :disabled="geoDisabled = true">
				OK
			</button>
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
		history_url: "http://api.openweathermap.org/data/2.5/onecall/timemachine"
	}),
	computed:
	{},
	methods:
	{}
}
</script>

<style scoped src="./Geolocation.css"></style>
