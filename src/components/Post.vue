<script setup>
import { onMounted, ref } from "vue";
const props = defineProps({
  title: String,
  imgSrc: String,
  userName: String,
  phoneNumber: String,
});
let verifiedImgSrc = ref("../assets/not-found.jpg");
let imgFound = ref(false);
let loading = ref(false);
onMounted(async () => {
  // debugger
  loading.value = true;
  //checks if the image loaded properly and if so assign the url to the verifiedImgSrc to be displayed later in the template
  await fetch(props.imgSrc).then((res) => {
    if (res.ok) {
      imgFound.value = true;
      verifiedImgSrc.value = res.url;
    }
    loading.value = false;
  });
});
</script>

<template>
  <v-skeleton-loader
    v-if="loading"
    class="mx-auto mt-4 w-100 loading-post"
    type="article, image"
    color="grey-darken-3"
  ></v-skeleton-loader>
  <div v-else class="post">
    <div class="user-info">
      <h2>{{ userName }}</h2>
      <a :href="`tel:${phoneNumber}`">{{ phoneNumber }}</a>
    </div>
    <h5>{{ title }}</h5>
    <img
      :src="imgFound ? verifiedImgSrc : '../../public/not-found.jpg'"
      alt="image for the post"
    />
  </div>
</template>

<style scoped>

.post {
  display: flex;
  flex-direction: column;
  gap: 8px;
  border-radius: 12px;
  padding: 12px;
  background-color: #363636;
  margin: 0px auto 16px;
}

.loading-post,
.post {
  min-width: 624px;
  max-width: 95%;
}


@media (max-width: 720px) {
  .post,
  .loading-post {
    min-width: 80%;
  }
}

.post img {
  border-radius: 10px;
}
</style>
