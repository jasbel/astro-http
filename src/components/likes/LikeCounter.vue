<template>
  <div v-if="isLoading">
    Loading...
  </div>
  <button v-else-if="likeCount===0" @click="likepost">Like this post</button>
  <button v-else @click="likepost">Like this post

    <span>{{ likeCount }}</span>
  </button>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import debounce from 'lodash.debounce';

interface Props {
  postId: string
}

const props = defineProps<Props>()

const likeCount = ref(0)
const likeClick = ref(0)
const isLoading = ref(true)

const likepost = () => {
  likeCount.value = likeCount.value+1
}

const getCurrentLikes = async () => {
  const resp = await fetch(`/api/likes/${props.postId}`)
  console.log(props.postId);
  console.log(resp);
  
  if(!resp.ok) return;

  const data = await resp.json()

  likeCount.value = data.likes;
  isLoading.value = false
}

getCurrentLikes()

</script>

<style scoped>
button {
  background-color: #5e51bc;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.3s;
}

button:hover {
  background-color: #4a3f9a;
}
</style>
