<template>
  <div class="subreddit-lane">
    <div class="header">
      <h2>{{ subreddit }}</h2>
      <button @click="removeLane" class="remove-btn">Remove</button>
    </div>

    <div v-if="loading" class="loading">Chargement...</div>
    <div v-if="error" class="error">{{ error }}</div>

    <ol v-if="posts.length">
      <li v-for="post in posts" :key="post.id" class="post">
        <h3>{{ post.title }}</h3>
        <p>Author: {{ post.author }}</p>
        <p>Upvotes: {{ post.ups }}</p>
      </li>
    </ol>
  </div>
</template>

<script lang="ts" setup>
import { ref, onMounted } from 'vue';
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
  subreddit: {
    type: String,
    required: true,
  },
});

const emit = defineEmits(['remove']);

const posts = ref<any[]>([]);
const loading = ref(true);
const error = ref('');

const fetchPosts = async () => {
  try {
    const response = await fetch(`https://www.reddit.com/r/${props.subreddit}.json`);
    const data = await response.json();
    posts.value = data.data.children.map((child: any) => child.data);
    loading.value = false;
  } catch (e) {
    error.value = `Failed to load subreddit: ${props.subreddit}`;
    loading.value = false;
  }
};

const removeLane = () => {
  emit('remove');
};


onMounted(fetchPosts);
</script>

<style scoped>
.subreddit-lane {
  background-color: white;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  padding: 20px;
  width: 300px;
  max-height: 80vh;
  overflow-y: auto;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h2 {
  font-size: 1.5rem;
  color: #333;
}

button.remove-btn {
  background-color: transparent;
  border: none;
  color: red;
  cursor: pointer;
}

button.remove-btn:hover {
  color: darkred;
}

.post {
  margin-bottom: 20px;
}

.post h3 {
  font-size: 1.2rem;
  margin: 0;
  color: #FF4500;
}

.post p {
  font-size: 0.9rem;
  color: #555;
}

.loading {
  color: #FF4500;
  font-weight: bold;
  text-align: center;
}

.error {
  color: red;
  font-weight: bold;
  margin-top: 10px;
}
</style>





  
  