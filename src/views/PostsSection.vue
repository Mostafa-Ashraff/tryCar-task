<script setup>
import Post from "../components/Post.vue";
import { onMounted, ref } from "vue";
let storedPageNumber = parseInt(localStorage.getItem("pageNumber"));
let pageNumber = ref(storedPageNumber ? storedPageNumber : 1);
let fullInfoPosts = ref([]);
let shownPosts = ref([]);
let errors = ref([]);

// calculate the number of posts to show depending on the page number selected from the navigation

function calculateShownPosts() {
  shownPosts.value = fullInfoPosts.value.slice(
    (pageNumber.value - 1) * 10,
    pageNumber.value * 10 - 1
  );
}

//triggered when the users clicks on a number from the pagination
async function handlePagination() {
  window.scrollTo({ top: 0, behavior: "smooth" });
  localStorage.setItem("pageNumber", pageNumber.value);
  calculateShownPosts();
}

onMounted(async () => {
  let users, posts;

  //get the users from the endpoint and stores it in the users variable
  await fetch("https://jsonplaceholder.typicode.com/users")
    .then((res) => res.json())
    .then((data) => (users = data))
    .catch((err) => (errors.value[0] = err.message));

  //get the posts from the endpoint and stores it in the posts variable

  await fetch("https://jsonplaceholder.typicode.com/posts")
    .then((res) => res.json())
    .then((data) => {
      posts = data;
    })
    .catch((err) => (errors.value[1] = err.message));

  //map over the posts array to get the user who wrote each post then returns the username, post id, user id, user phone number,post title
  //as an object to be stored in the fullInfoPosts array
  fullInfoPosts.value = posts.map((post, i) => {
    let authorOfPost = users.find((user) => user.id === post.userId);
    let userPhone = authorOfPost.phone.split(" ")[0];
    return {
      id: post.id,
      userId: authorOfPost.id,
      userName: authorOfPost.name,
      userPhone,
      title: post.title,
    };
  });
  calculateShownPosts();
});
</script>

<template>
  <template v-if="errors">
    <v-alert
      type="error"
      v-for="error in errors"
      :key="error"
      class="ma-4 t-0"
      :text="error"
    ></v-alert>
  </template>
  <section class="d-flex flex-column justify-center align-center mx-auto">
    <Post
      class="m-auto"
      v-for="post in shownPosts"
      :key="post.id"
      :title="post.title"
      :userName="post.userName"
      :imgSrc="`https://picsum.photos/600/300/?image=${post.id}`"
      :phoneNumber="post.userPhone"
    />
  </section>
  <v-pagination
    v-model="pageNumber"
    :length="fullInfoPosts.length / 10"
    @update:modelValue="handlePagination"
    next-icon="mdi-menu-right"
    prev-icon="mdi-menu-left"
  ></v-pagination>
</template>
