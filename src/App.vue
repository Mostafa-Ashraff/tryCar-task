<script setup>
import Post from "./components/Post.vue";
import axios from "axios";
import { onMounted, ref } from "vue";

let fullInfoPosts = ref([]);
onMounted(async () => {
  let users, posts;

  await fetch("https://jsonplaceholder.typicode.com/users")
    .then((res) => res.json())
    .then((data) => (users = data))
    .catch((err) => console.log(err));

  await fetch("https://jsonplaceholder.typicode.com/posts")
    .then((res) => res.json())
    .then((data) => (posts = data));
  // console.log(posts);

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
    // console.log(authorOfPost);
    // fullInfoPosts.value[i] = fullInfoPost;
  });

  console.log(fullInfoPosts.value);
});
</script>

<template>
  <Post
    v-for="post in fullInfoPosts"
    :key="post.id"
    :title="post.title"
    :userName="post.userName"
    :imgSrc="`https://picsum.photos/600/300/?image=${post.id}`"
    :phoneNumber="post.userPhone"
  />
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
