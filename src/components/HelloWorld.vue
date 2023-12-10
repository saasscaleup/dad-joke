<script setup>
defineProps({
  msg: {
    type: String,
    required: true
  }
})
</script>

<template>
  <div class="greetings">
    <h1 class="purple">{{ msg }}</h1>
    <h3>
      Welcome to <a href="https://youtube.com/@ScaleUpSaaS" target="_blank" rel="noopener">ScaleUp-Saas</a> Aws Bedrock tutorial. Please 
      <a href="https://vitejs.dev/" target="_blank" rel="noopener">Like</a> +
      <a href="https://vuejs.org/" target="_blank" rel="noopener">Subscribe</a> ✅🫶
    </h3>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

.purple, a{
  color: rgb(255, 0, 255);
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
