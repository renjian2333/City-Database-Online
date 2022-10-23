<script setup>
import Card from './Card.vue'
import axios from 'axios'
import Bus from '../utils/Bus'
import NewsItem from './NewsItem.vue'
import { reactive } from 'vue'

const newslist = reactive({
    data: []
})
const state = reactive({
    pageSize: 6,
    currentPage: 1,
    total: 50
})
Bus.on('cityName', (value) => {
    axios.get("http://api.tianapi.com/travel/index?key=5bb158c4e1e26656f07e30fa705f0767&num=50&word=" + value)
        .then((reponse) => {
            newslist.data = reponse.data.newslist;
            state.total = newslist.data.length;
            state.currentPage = 1;
        })
})

const pageChange = (currentPage)=>{
    state.currentPage = currentPage;
}

</script>

<template>
    <Card>
        <template #title>News</template>
        <NewsItem v-for="news in newslist.data.slice((state.currentPage-1)*state.pageSize, state.currentPage*state.pageSize)" :key="news.id"
            :picUrl="news.picUrl" :href="news.url">
            <template #title> {{news.title}}</template>
            <template #description> {{news.description}}</template>
        </NewsItem>
        <div class="pagination">
        <el-pagination layout="prev, pager, next" :total="state.total" :current-page="state.currentPage"
            :page-size="state.pageSize" @current-change="pageChange"/>
        </div>
    </Card>
</template>

<style scoped>
.notice {
    color: gray;
    font-size: 12px;
}

.pagination {
    width: 19.5rem;
    margin: 0 auto;
}
</style>