<script setup>
defineProps({
  message: {
    type: String,
    required: true,
  }
})
</script>

<template>
  <div class="error-box">
    <b>Error:</b> {{ message }}
  </div>
</template>

<style scoped>
.error-box {
  background-color: #e59898;
  border: 1px solid #b33f3f;
  color: #681515;
}
</style>