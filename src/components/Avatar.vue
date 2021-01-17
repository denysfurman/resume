<template>
  <div class="avatar">
    <img :src="url">
  </div>
</template>

<script>
export default {
  name: "Avatar",
  props: ['url']

}
</script>

<style scoped>

</style>
