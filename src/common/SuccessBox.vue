<script setup>
defineProps({
  message: {
    type: String,
    required: true,
  }
})
</script>

<template>
  <div class="success-box">
    {{ message }}
  </div>
</template>

<style scoped>
.success-box {
  background-color: #b9d6a9;
  border: 1px solid #467030;
  color: #467030;
}
</style>