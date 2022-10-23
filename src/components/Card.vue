<script setup>
const props = defineProps(['href'])
</script>
<template>
  <el-card shadow="always" class="card">
    <h3>
      <slot name="title"></slot>
    </h3>
    <slot></slot>
    <a :href="href" target="_blank"><slot name="from"></slot></a>
  </el-card>
</template>

<style scoped>
h3 {
  color: #3d719c;
  font-weight: bold;
}

a {
  font-size: 12px;
  font-weight: 500;
  font-style: italic;
}

.card {
  margin-bottom: 0.5rem;
}

</style>