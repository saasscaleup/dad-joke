<template>
    <div class="generate-image-container">
      <h2 class="title">📩 Generate Content for your EMAIL MARKETING 📩</h2>
      <div class="input-container">
        <textarea v-model="topic" placeholder="Enter a topic" class="input" />
        <button @click="generateText" class="button" :disabled="isLoading">
          <span v-if="isLoading">Loading...</span>
          <span v-else>Generate Text</span>
        </button>
      </div>
      <div v-if="generate_content" class="generate-image-result">
        <p class="generate_content-text">{{ generate_content }}</p>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios'
  
  export default {
    data() {
      return {
        topic: '',
        generate_content: null,
        isLoading: false // Added isLoading variable
      }
    },
    methods: {
      async generateText() {
        this.isLoading = true // Show loader
        this.generate_content = null
        let prompt = this.topic
        try {
          const response = await axios.get(
            'https://ji4oufomz7m4qhno5f4i6jjqhe0tpwbm.lambda-url.us-east-1.on.aws/?prompt='+prompt+'&type=text',
            {
              headers: {
                'Content-Type': 'application/json',
              }
            }
          )
            console.log(response.data);
          this.generate_content = response.data.completions[0].data.text;
        } catch (error) {
          console.error('Error fetching Dad generate_content:', error)
          this.generate_content = 'Error fetching generate_content'
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
    border: 2px solid #ff6f61;
    border-radius: 10px;
    background-color: #b8ffb3;
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
    width: 400px;
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
  
  .generate-image-result {
    margin-top: 20px;
    font-size: 1.5em;
    color: #ff6f61;
    background-color: #fff;
    padding: 10px;
    border-radius: 5px;
    border: 2px solid #ff6f61;
  }
  
  .generate_content-text {
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
  