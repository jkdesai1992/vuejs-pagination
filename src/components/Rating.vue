<template>
  <div>
    <span class="rate" v-for="(item, index) in 5" v-bind:key="index">
      <i v-if="ratingStars >= item" class="text-success">*</i>
      <i v-else class="fas fa-star">*</i>
    </span>
  </div>
</template>

<script>
export default {
  name: "Rating",
  props: ["ratingStars"]
};
</script>

<style scoped>
.text-success {
  color: rgb(3, 85, 17);
}
</style>
