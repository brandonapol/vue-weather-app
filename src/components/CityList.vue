<template>
    <div v-for="city in savedCities" :key="city.id">
        <CityCard :city="city" @click="goToCityView(city)" />
    </div>

    <p v-if="savedCities.length === 0">
        No locations added. To start tracking a location, 
        search in the field above.
    </p>
</template>

<script setup>
import axios from "axios";
import { ref } from "vue";
import { useRouter } from "vue-router";
import CityCard from "./CityCard.vue";

const savedCities = ref([]);
const getCities = async () => {
    if (localStorage.getItem('savedCities')) {
        savedCities.value = JSON.parse(
            localStorage.getItem("savedCities"))
    };

    const requests = [];
    savedCities.value.forEach((city) => {
        requests.push(
            axios.get(
                `https://api.openweathermap.org/data/2.5/onecall?lat=${route.query.lat}&lon=${route.query.lng}&exclude={part}&appid=cb0e2c5ea4c83b9c517301b8e88f8121&units=imperial`
            )
        );
    });

    const weatherData = await Promise.all(requests);

    weatherData.forEach((value, index) => {
        savedCities.value[index].weather = value.data;
    });
};
await getCities();

const router = useRouter();
const goToCityView = (city) => {
    router.push({
        name: "cityView",
        params: { state: city.state, city: city.city },
        query: {
            lat: city.coords.lat,
            lng: city.coords.lng,
        },
    });
};
</script>

