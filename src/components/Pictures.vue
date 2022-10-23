<script setup>
import { createClient } from 'pexels';
import { reactive, ref, onMounted, nextTick } from 'vue'
import Card from './Card.vue'
import { ArrowRightBold, ArrowLeftBold } from '@element-plus/icons-vue'
import Bus from '../utils/Bus'
import pinyin from 'js-pinyin'

const client = createClient('563492ad6f91700001000001c3a0510dee2f455f8708d57a9e279cf2');
const scrollbarRef = ref()
const innerRef = ref()
const state = reactive({
    urls: [],
    refs:[],
    len: 0,
    totalLen: 0,
    increment: 300,
    href: "",
    lflag: false,
    rflag: false
})

Bus.on('cityName', (value) => {
    let newQuery = value;
    if (/^[\u4e00-\u9fa5]*$/.test(value)) {
        newQuery = pinyin.getFullChars(value);
        if (newQuery.toLowerCase() === "zhongqing") {
            newQuery = "ChongQing";
        } else if (newQuery.toLowerCase() === "xicang") {
            newQuery = "Tibet";
        } else if (newQuery.toLowerCase() === "xianggang"){
            newQuery = "HongKong"
        }
        state.href = "https://www.pexels.com/search/" + newQuery;
    }
    client.photos.search({ query: newQuery, per_page: 30 }).then((response) => {
        state.urls = []
        response.photos.forEach(element => {
            state.urls.push(element.src.medium)
        });
        initScroll();
    })
})

onMounted(() => {
    state.increment = innerRef.value.clientWidth;
})

const initScroll = ()=>{
    setTimeout(()=>{
        var array = Array.from(innerRef.value.children);
        state.totalLen = 0;
        array.forEach(element => {
            state.totalLen += element.clientWidth;
        });
        state.totalLen -= state.increment;
        if(state.totalLen > 0){
            state.rflag = true;
        }
    },1000)
}

const scrollTo = (n) => {
    state.len += n;
    if (state.len <= 0) {
        state.len = 0;
        state.lflag = false;
        if(state.totalLen > 0){
            state.rflag = true;
        }
    } else { 
        state.lflag = true;
        if(state.len >= state.totalLen){
            state.len = state.totalLen;
            state.rflag = false;
        } else{
            state.rflag = true;
        }
    }
    scrollbarRef.value.setScrollLeft(state.len);
}

</script>

<template>
    <Card :href="state.href">
        <template #title>Pictures</template>
        <div class="box">
            <el-scrollbar ref="scrollbarRef">
                <div class="scrollbar-flex-content" ref="innerRef" id="images">
                    <el-image v-for="(url, index) in state.urls" :key="url" :src="url" :preview-src-list="state.urls" :initial-index="index"
                        class="scrollbar-demo-item" delay />
                </div>
            </el-scrollbar>

            <div class="scroll-left" @click="scrollTo(-state.increment)" v-if="state.lflag">
                <el-button class="button" :icon="ArrowLeftBold" circle />
            </div>
            <div class="scroll-right" @click="scrollTo(state.increment)" v-if="state.rflag">
                <el-button class="button" :icon="ArrowRightBold" circle />
            </div>
        </div>
        <template #from>pexels.com</template>
    </Card>

</template>

<style scoped>
.scrollbar-flex-content {
    display: flex;
}

.scrollbar-demo-item {
    flex-shrink: 0;
    height: 200px;
    margin-right: 10px;
    border-radius: 4px;
}

.box {
    position: relative;
}

.scroll-right {
    position: absolute;
    z-index: 999;
    right: 0;
    top: 50%;
    transform: translate(20%, -50%);
}

.scroll-left {
    position: absolute;
    z-index: 999;
    left: 0;
    top: 50%;
    transform: translate(-20%, -50%);
}
</style>
