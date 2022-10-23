<script setup>
import { Search } from '@element-plus/icons-vue'
import { ref, onMounted } from 'vue'
import Bus from '../utils/Bus'

const input = ref("上海")
onMounted(() => {
    init(input.value);
})

const onSearch = () => {
    init(input.value);
}

const init = (input) => {
    Bus.emit('cityName', input);
    fetch(
        "https://restapi.amap.com/v3/geocode/geo?key=bad0cfcf0596bd207315c7334988b85d&address=" +
        input
    )
        .then(function (response) {
            return response.json();
        })
        .then((res) => {
            Bus.emit('location',res.geocodes[0].location)
        });
}


</script>
<template>
    <div class="border">
        <el-row type="flex">
            <el-col :span="16">
                <el-input v-model="input" placeholder="Enter a chinese city name" @change="onSearch"></el-input>
            </el-col>
            <el-col :span="2">
                <el-button type="primary" :icon="Search" @click="onSearch">Search
                </el-button>
            </el-col>
        </el-row>
    </div>

</template>

<style scoped>
.border {
    display: block;
    width: 28rem;
}

:deep(.el-input__inner) {
    height: 2.5rem;
}

.el-button {
    background-color: #3d719c;
    border: none;
    height: 2.5rem;
}
</style>
