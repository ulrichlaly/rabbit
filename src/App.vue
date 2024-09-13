<template>
  <div class="app">
    <h1>Reddit Client</h1>
    <div class="input-container">
      <input v-model="newSubreddit" placeholder="Entrer subreddit" @keyup.enter="addSubreddit" />
      <button @click="addSubreddit">AJOUTER</button>
    </div>

    <div v-if="error" class="error">{{ error }}</div>

    <div class="lanes">
      <SubredditLane
        v-for="(subreddit, index) in subreddits"
        :key="subreddit"
        :subreddit="subreddit"
        @remove="removeSubreddit(index)"
      />
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, onMounted } from 'vue';
import SubredditLane from './components/SubredditLane.vue';

const subreddits = ref<string[]>([]);
const newSubreddit = ref('');
const error = ref('');

const addSubreddit = () => {
  if (!newSubreddit.value.trim()) return;
  subreddits.value.push(newSubreddit.value);
  localStorage.setItem('subreddits', JSON.stringify(subreddits.value));
  newSubreddit.value = '';
};

const removeSubreddit = (index: number) => {
  subreddits.value.splice(index, 1);
  localStorage.setItem('subreddits', JSON.stringify(subreddits.value));
};

const restoreSubreddits = () => {
  const storedSubreddits = localStorage.getItem('subreddits');
  if (storedSubreddits) {
    subreddits.value = JSON.parse(storedSubreddits);
  }
};

onMounted(() => {
  restoreSubreddits();
});
</script>




<style scoped>
.app {
  font-family: 'Arial', sans-serif;
  padding: 20px;
  text-align: center;
}

h1 {
  font-size: 2.5rem;
  color: #FF4500;
}

.input-container {
  margin: 20px 0;
}

input {
  padding: 10px;
  width: 250px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-right: 10px;
}

button {
  padding: 10px 15px;
  background-color: #FF4500;
  border: none;
  color: white;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #ff5700;
}

.lanes {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
}

.error {
  color: red;
  font-weight: bold;
  margin-top: 10px;
}
</style>






