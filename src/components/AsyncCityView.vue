<template>
    <div>

    </div>
</template>

<script setup>
import axios from "axios";
import { useRoute } from "vue-router";

const route = useRoute();
const getWeatherData = async () => {
    try {
        const weatherData = await axios.get(
        `https://api.openweathermap.org/data/2.5/onecall?lat=${route.query.lat}&lon=${route.query.lng}&exclude={part}&appid=cb0e2c5ea4c83b9c517301b8e88f8121&units=imperial`
        );
        
        // calculate current date and time
        const localOffset = new Date().getTimezoneOffset() * 600000;
        const utc = weatherData.data.current.dt * 1000 + localOffset;
        weatherData.data.currentTime = 
        utc + 1000 * weatherData.data.timezone_offset;

        //hourly weather offset
        weatherData.data.hourly.forEach((hour) => {
            const utc = hour.dt * 1000 + localOffset;
            hour.currentTime = 
            utc + 1000 * weatherData.data.timezone_offset;
        });

        return weatherData.data;
    } catch (err) {
        console.log(err);
    }
};

const weatherData = await getWeatherData();
console.log(weatherData)
</script>

