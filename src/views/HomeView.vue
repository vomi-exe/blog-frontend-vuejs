<template>
  <div>
    <h1>Home</h1>
    <div v-if="error" style="color: red">{{ error }}</div>
    <div v-if="posts.length">
      <PostList :posts="posts" />
    </div>
    <div v-else style="color: green">Loading...</div>
  </div>
</template>

<script>

import { ref } from 'vue';
import PostList from "../components/PostList.vue";

export default {
  name: "About",
  components: { PostList },
  setup() {
    const posts = ref([])
    const error = ref(null);

    const load = async () => {
      try {
        let data = await fetch("http://localhost:3000/posts");
        if (!data.ok) {
          throw Error("no data available");
        }
        posts.value = await data.json();

      } catch (err) {
        error.value = err.message;
        console.log(err.value);
      }
    }
    load();

    return { posts, error }
  }
}
</script>

<style></style>