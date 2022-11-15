<script setup>
import AMapLoader from '@amap/amap-jsapi-loader'
import Bus from '../utils/Bus'
import { reactive, watch } from 'vue'
import Card from './Card.vue'

const state = reactive({
    center: [106.55, 29.55],
    href: ""
});

Bus.on('location', (value) => {
    state.center = value.split(",");
})

Bus.on('cityName',(value) => {
    state.href = "https://ditu.amap.com/search?query=" + value
})

AMapLoader.load({
    "key": "yourkey",
    "version": "2.0",
    "plugins": [], 
}).then((AMap) => {
    let map = new AMap.Map('map', {
        center: state.center,
        zoom: 4,
    });
    let markerPoint = new AMap.Marker({
        position: map.getCenter(), 
        draggable: false,
        defaultCursor: 'pointer'
    });
    map.add(markerPoint);

    watch(state, () => {
        map.setCenter(state.center)
        markerPoint.setPosition(state.center)
    })

}).catch(e => {
    console.log(e);
})
</script>

<template>
    <Card :href="state.href" website="amap.com">
        <div id="map"></div>
    </Card>
</template>
      
<style scoped>
#map {
    width: 100%;
    height: 20rem;
}
</style>
