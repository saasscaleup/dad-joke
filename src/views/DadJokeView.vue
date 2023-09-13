<template>
  <div class="dad-joke-container">
    <h2 class="title">🤣 Get a Dad Joke 🤣</h2>
    <div class="input-container">
      <input v-model="topic" placeholder="Enter a topic" class="input" />
      <button @click="getDadJoke" class="button" :disabled="isLoading">
        <span v-if="isLoading">Loading...</span>
        <span v-else>Get Joke</span>
      </button>
    </div>
    <div v-if="joke" class="dad-joke-result">
      <p class="joke-text">{{ joke }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

const VITE_CHATGPT_TOKEN = import.meta.env.VITE_CHATGPT_TOKEN || 'any-default-local-build_env'

export default {
  data() {
    return {
      topic: '',
      joke: null,
      isLoading: false // Added isLoading variable
    }
  },
  methods: {
    async getDadJoke() {
      this.isLoading = true // Show loader
      this.joke = null
      let propt =
        'Think about you as the funniest person ever!!!  I will give you a topic and you will generate me a dad joke about this topic. the topic is: ' +
        this.topic
      try {
        const response = await axios.post(
          'https://api.openai.com/v1/chat/completions',
          {
            model: 'gpt-3.5-turbo-16k',
            messages: [{ role: 'user', content: propt }]
          },
          {
            headers: {
              'Content-Type': 'application/json',
              Authorization: 'Bearer ' + VITE_CHATGPT_TOKEN
            }
          }
        )

        this.joke = response.data.choices[0].message.content
      } catch (error) {
        console.error('Error fetching Dad joke:', error)
        this.joke = 'Error fetching joke'
      } finally {
        this.isLoading = false // Hide loader
      }
    }
  }
}
</script>

<style scoped>
.dad-joke-container {
  text-align: center;
  margin: 20px;
  padding: 20px;
  border: 2px solid #ff6f61;
  border-radius: 10px;
  background-color: #ffecb3;
}

.title {
  font-size: 2em;
  margin-bottom: 20px;
  color: #ff6f61;
}

.input-container {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.input {
  padding: 10px;
  border-radius: 5px;
  border: 2px solid #ff6f61;
  margin-right: 10px;
}

.button {
  padding: 10px 20px;
  border-radius: 5px;
  background-color: #ff6f61;
  color: #fff;
  border: none;
  cursor: pointer;
}

.dad-joke-result {
  margin-top: 20px;
  font-size: 1.5em;
  color: #ff6f61;
  background-color: #fff;
  padding: 10px;
  border-radius: 5px;
  border: 2px solid #ff6f61;
}

.joke-text {
  margin: 0;
}

.button {
  position: relative;
}

.button span {
  visibility: visible;
}

.button span.loader {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  visibility: hidden;
}
</style>
