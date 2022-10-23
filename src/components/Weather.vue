<script setup>
import Card from './Card.vue'
import axios from 'axios'
import Bus from '../utils/Bus'
import { reactive } from 'vue'
import WeatherDescCard from './WeatherDescCard.vue'

const state = reactive({
    temp: "17",
    icon: "100",
    feelsLike: "17",
    text: "多云",
    humidity: "23",
    windScale: "2",
    windSpeed: "3",
    windDir: "南风",
    vacation: {
        category: "",
        text: ""
    }
})
Bus.on('location', (value) => {
    axios.get("https://devapi.qweather.com/v7/weather/now?key=4517580d69e54c8e83ee40df5a938c37&location=" + value).then((reponse) => {
        let data = reponse.data.now;
        state.temp = data.temp;
        state.feelsLike = data.feelsLike;
        state.humidity = data.humidity;
        state.icon = "qi-" + data.icon;
        state.text = data.text;
        state.windDir = data.windDir;
        state.windScale = data.windScale;
        state.windSpeed = data.windSpeed;
    })

    axios.get("https://devapi.qweather.com/v7/indices/1d?key=4517580d69e54c8e83ee40df5a938c37&type=6&location=" + value).then((response) => {
        let data = response.data.daily[0];
        state.vacation.category = data.category;
        state.vacation.text = data.text;
    })
})


</script>
<template>
    <Card>
        <div class="wrapper">
            <div>
                <i class="weather-icon" :class="state.icon"></i>
            </div>

            <div>
                <span class="temp">{{state.temp}}</span> &#8451;
            </div>

            <div class="vacation">
                <p>旅游指数：{{state.vacation.category}}</p>
                <p class="vacationDesc">{{state.vacation.text}}</p>
            </div>
        </div>
        <el-divider />

        <div class="wrapper2">
            <WeatherDescCard>
                <template #name>天气</template>
                <template #desc>{{state.text}}</template>
            </WeatherDescCard>
            <WeatherDescCard>
                <template #name>体感温度</template>
                <template #desc>{{state.feelsLike}}&#8451</template>
            </WeatherDescCard>
            <WeatherDescCard>
                <template #name>湿度</template>
                <template #desc>{{state.humidity}}</template>
            </WeatherDescCard>
            <WeatherDescCard>
                <template #name>风向</template>
                <template #desc>{{state.windDir}}</template>
            </WeatherDescCard>
            <WeatherDescCard>
                <template #name>风力级别</template>
                <template #desc>{{state.windScale}}</template>
            </WeatherDescCard>
            <WeatherDescCard>
                <template #name>风速</template>
                <template #desc>{{state.windSpeed}}km/h</template>
            </WeatherDescCard>
            
        </div>
    </Card>
</template>

<style scoped>
.weather-icon {
    color: gray;
    font-size: 3.5rem;
}

.wrapper {
    display: grid;
    grid-template-columns: 1fr 1.1fr 3.8fr;
    color: #3d719c;
    text-align: center;
}

.wrapper2 {
    display: grid;
    grid-template-columns: repeat(3,1fr);
    grid-template-rows: repeat(2,2.6rem);
    text-align: center;
    padding-top: 0.5rem;
}
.temp {
    font-size: 3.5rem;
}

.vacation {
    padding:1rem 0.5rem 0 1rem;
    text-align: left;
}

.vacationDesc {
    font-size: 12px;
    font-weight: 500;
    color:gray;
}

:deep(.el-divider--horizontal){
    margin: 8px;
    width: auto;
}
</style>