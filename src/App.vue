<script setup>
import ImageContainer from './components/ImageContainer.vue';
</script>

<template>
  <header>
    <div class="wrapper">
      <div>
        <ImageContainer />
      </div>
    </div>
  </header>
</template>

<style scoped>
header {
  line-height: 1.5;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
