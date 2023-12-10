<template>
  <div class="generate-image-container">
    <h2 class="title">🎇 Generate Images for FREE 🎇</h2>
    <div class="input-container">
      <input v-model="topic" placeholder="Enter a topic" class="input" />
      <button @click="generateImage" class="button" :disabled="isLoading">
        <span v-if="isLoading">Loading...</span>
        <span v-else>Generate Image</span>
      </button>
    </div>
    <div v-if="base64_image" class="generate-image-result">
      <img v-bind:src=" this.base64_image " /> 
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      topic: '',
      base64_image: null,
      isLoading: false // Added isLoading variable
    }
  },
  methods: {
    async generateImage() {
      this.isLoading = true // Show loader
      this.base64_image = null
      let prompt = this.topic
      try {
        const response = await axios.get(
          'https://ji4oufomz7m4qhno5f4i6jjqhe0tpwbm.lambda-url.us-east-1.on.aws/?prompt='+prompt+'&type=image',
          {
            headers: {
              'Content-Type': 'application/json',
            }
          }
        )
          console.log(response.data);
        this.base64_image = 'data:image/jpg;base64,'+response.data.artifacts[0].base64
      } catch (error) {
        console.error('Error fetching Dad base64_image:', error)
        this.base64_image = 'Error fetching base64_image'
      } finally {
        this.isLoading = false // Hide loader
      }
    }
  }
}
</script>

<style scoped>
.generate-image-container {
  text-align: center;
  margin: 20px;
  padding: 20px;
  border: 2px solid #6183ff;
  border-radius: 10px;
  background-color: #76dffc;
}

.title {
  font-size: 2em;
  margin-bottom: 20px;
  color: #6183ff;
}

.input-container {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.input {
  padding: 10px;
  width: 400px;
  border-radius: 5px;
  border: 2px solid #6183ff;
  margin-right: 10px;
}

.button {
  padding: 10px 20px;
  border-radius: 5px;
  background-color: #6183ff;
  color: #fff;
  border: none;
  cursor: pointer;
}

.generate-image-result {
  margin-top: 20px;
  font-size: 1.5em;
  color: #6183ff;
  background-color: #fff;
  padding: 10px;
  border-radius: 5px;
  border: 2px solid #6183ff;
}

.base64_image-text {
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
