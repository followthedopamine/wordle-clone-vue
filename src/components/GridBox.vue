<script setup>
import { defineProps } from "vue";
const props = defineProps({
  value: String,
});
</script>

<template>
  <span>{{ props.value }}</span>
</template>

<style scoped>
span {
  display: flex;
  width: 50px;
  height: 50px;
  margin: 4px;
  justify-content: center;
  align-items: center;
  border: 2px solid var(--dark-grey);
}
</style>
