<template>
	<div id="app" style="height: 100vh; width: 100vw;">
		<img alt="Vue logo" width="150" src="https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fblr.vue.community%2Flogo.png&f=1&nofb=1&ipt=96e6acdff50265cddc4e4f1871e54c8cde89cbac7cae602b86a050f108b39d89&ipo=images">
		
		<HelloWorld msg="The-philippine-address | Composition Api"/>

		<select class="select" @change="handleProvince">
			<option disabled selected>Select Region</option>
			<option v-for="region in regionsArr.value" :value="region.region_code" :key="region.region_code">{{
				region.region_name
				}}
			</option>
		</select>

		<br/><br/>

		<select class="select" @change="handleCity">
			<option disabled selected>Select Province</option>
			<option v-for="province in provinceArr.value" :value="province.province_code" :key="province.province_code">
				{{ province.province_name }}
			</option>
		</select>

		<br/><br/>

		<select class="select" @change="handleBarangay">
			<option disabled selected>Select City</option>
			<option v-for="city in citiesArr.value" :value="city.city_code" :key="city.city_code">{{ city.city_name }}</option>
		</select>

		<br/><br/>

		<select class="select" @change="barangaysArrChange">
			<option disabled selected>Select Barangay</option>
			<option v-for="barangay in barangaysArr.value" :value="barangay.brgy_code" :key="barangay.brgy_code">{{
				barangay.brgy_name
				}}
			</option>
		</select>

		<br/><br/>

		<p>{{ barangay }} <span v-if="barangay">,</span> {{ city }}<span v-if="province && city">,</span> {{ province }}<span v-if="region && province">,</span> {{ region }}</p>
		
		<a style="margin-top: 20vh;" href="https://www.github.com/shuashuaa/the-philippine-address">select-philippines-address in vuejs3 composition api by shuashuaa</a>

	</div>
</template>

<script setup>
import { ref, reactive, onBeforeMount, onMounted } from 'vue'
import HelloWorld from './components/HelloWorld.vue'
// eslint-disable-next-line no-unused-vars
import {regions, provinces, cities, barangays} from 'select-philippines-address';

const regionsArr = reactive([]);
const region = ref(null);
const province = ref(null);
const city = ref(null);
const barangay = ref(null);

const getRegions = onBeforeMount( async() => {
	const response = await regions();
	console.log(response)
	regionsArr.value = response;
})

const provinceArr =  reactive([]);

const handleProvince = (e) => {
	region.value = e.target.selectedOptions[0].text;
		provinces(e.target.value).then(response => {
		provinceArr.value = response;
	});
}

const citiesArr =  reactive([]);

const handleCity = (e) => {
	province.value = e.target.selectedOptions[0].text;
		cities(e.target.value).then(response => {
		citiesArr.value = response;
	});
}

const barangaysArr =  reactive([]);

const handleBarangay = (e) => {
	city.value = e.target.selectedOptions[0].text;
		barangays(e.target.value).then(response => {
		barangaysArr.value = response;
	});
}

const barangaysArrChange = (e) => {
	barangay.value = e.target.selectedOptions[0].text;
}

</script>

<style>

html { overflow: hidden !important; }

.select:not(:last-child) {
  margin-bottom: 10px;
  width: 30vw;
}

#app {
	display: flex !important;
	/* justify-content: center !important; */
	flex-direction: column !important;
	align-items: center !important;
	margin-top: 10vh;
	font-family: "IBM Plex Mono", monospace;
	font-weight: 700;
	font-optical-sizing: auto !important;
	font-style: normal !important;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
}

@import url('https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;1,100;1,200;1,300;1,400;1,500;1,600;1,700&display=swap');
</style>