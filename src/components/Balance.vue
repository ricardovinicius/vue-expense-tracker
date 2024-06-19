<template>
  <h4>Your balance</h4>
  <h1 id="balance">{{ total }}</h1>
</template>

<script setup>
  const props = defineProps({
    total: {
      type: Number,
      required: true,
    },
  });
</script>