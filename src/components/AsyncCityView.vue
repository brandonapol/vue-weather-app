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
            ``
            // TODO: Work on this later
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
</script>

<style lang="scss" scoped>

</style>