<script setup>
const props = defineProps(['href', 'picUrl'])
</script>

<template>
    <div class="wrapper">
        <div>
            <el-image class="image" :src="picUrl">
            </el-image>
        </div>
        <div class="news-content">
            <a :href="href" target="_blank">
                <h3>
                    <slot name="title"></slot>
                </h3>
            </a>
            <br />
            <p>
                <slot name="description"></slot>
            </p>
        </div>
    </div>
    <el-divider />
</template>

<style scoped>
.wrapper {
    display: grid;
    grid-template-columns: auto auto;
    column-gap: 0.6rem;
}

.image {
    width: 10rem;
    height: 6rem;
    border-radius: 4px;
}

.news-content {
    text-align: left;
}

.el-divider {
    margin: 4px;
}
</style>