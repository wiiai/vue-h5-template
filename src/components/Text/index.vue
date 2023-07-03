<template>
  <span>{{ content }}</span>
</template>

<script lang="ts" setup>
  defineProps<{
    content: string;
  }>();
</script>
