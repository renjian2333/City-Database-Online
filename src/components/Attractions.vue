<script setup>
import axios from 'axios';
import Bus from '../utils/Bus'
import Card from './Card.vue'
import { reactive } from 'vue'


const attractions = reactive({
    data: []
})

Bus.on('cityName', (value) => {
    axios.get("https://restapi.amap.com/v3/place/text?offset=6&key=bad0cfcf0596bd207315c7334988b85d&types=110202&city=" + value).then((response) => {
        attractions.data = []
        response.data.pois.forEach(element => {
            let tmp = {
                name: element.name,
                photo: element.photos[0].url,
                link: "https://www.bing.com/search?q=" + element.name
            };
            attractions.data.push(tmp);
        });
    }) 
})

</script>
<template>
    <Card>
        <template #title>Attractions</template>
        <div class="wrapper">
            <div v-for="attraction in attractions.data" :key="attraction.name" class="item">
                <a :href="attraction.link" target="_blank">
                    <el-image :src="attraction.photo">
                    </el-image>
                    <p>{{attraction.name}}</p>
                </a>
            </div>
        </div>
    </Card>
</template>

<style>
.wrapper {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    column-gap: 0.5rem;
    text-align: center;
}

.el-image {
    height: 80px;
    border-radius: 3px;
}
</style>