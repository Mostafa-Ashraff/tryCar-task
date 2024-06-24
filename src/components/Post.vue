<script setup>

defineProps({
  title: String,
  imgSrc: String,
  userName: String,
  phoneNumber: String,
})

</script>

<template>
  <h2>{{ userName }}{{ phoneNumber }}</h2>
  <h4>{{ title }}</h4>
  <img :src="imgSrc" alt="">

</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
