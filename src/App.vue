<script setup>
import SearchBox from './components/SearchBox.vue'
import General from './components/General.vue'
import Pictures from './components/Pictures.vue'
import Map from './components/Map.vue'
import Weather from './components/Weather.vue'
import News from './components/News.vue'
import Attractions from './components/Attractions.vue'
</script>

<template>
  <header>
    <div class="search_pos">
      <h1 class="title">City Database</h1>
      <SearchBox />
    </div>
  </header>

  <main>
    <div class="left">
      <General />
      <Pictures />
      <News />
    </div>

    <div class="right">
      <Weather />
      <Map />
      <Attractions />
    </div>

  </main>
</template>

<style scoped>
header {
  width: 100%;
  height: 200px;
  padding-top: 30px;
  background-image: url("src\\assets\\bg.jpg");
  background-size: cover;
}

.title {
  font-weight: 700;
  color: #3d719c;
  font-size: 4rem;
}

.search_pos {
  width: 70rem;
  margin: 0 auto;
}

main {
  width: 70rem;
  margin: 0 auto;
  margin-top: 20px;
  display: grid;
  grid-template-columns: 1.5fr 1fr;
}

.left {
  padding-right: 1rem;
}

:deep(.el-card__body) {
  padding: 8px;
}
</style>
