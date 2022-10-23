<script setup>
import Bus from '../utils/Bus'
import Card from './Card.vue'
import { reactive } from 'vue'
import axios from 'axios'

const state = reactive({
  description:"",
  extract:"",
  content_url:""
})

Bus.on('cityName', (value) => {
  axios.get("https://zh.wikipedia.org/api/rest_v1/page/summary/" + value).then((response)=>{
    state.description = response.data.description;
    state.extract = response.data.extract;
    state.content_url = response.data.content_urls.desktop.page;
  })
})


</script>
<template>
  <Card :href="state.content_url">
    <template #title>General</template>
    <p class="sub-title">{{state.description}}</p>
    <p class="content">
      {{state.extract}}
    </p>
    <template #from>wikipedia.org</template>
  </Card>
</template>

<style scoped>
.sub-title {
  line-height: 2;
  color: #3d719c;
  font-style: oblique;
  font-size:11px;
}

.content {
  line-height: 1.3;
  font-size:13px;
  text-indent: 2rem;
}
</style>