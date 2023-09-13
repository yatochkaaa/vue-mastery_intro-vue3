<template>
  <ul>
    <li v-for="detail in details" :key="detail">{{ detail }}</li>
  </ul>
</template>
<script>
export default {
  props: {
    details: {
      type: Object,
      required: true,
    },
  },
};
</script>
<style scoped></style>
