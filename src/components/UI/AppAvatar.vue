<template>
  <div class="avatar">
    <img :src="value" />
    <div class="delete" @click="$emit('delete')">
      &#10060;
    </div>
  </div>
</template>

<script>
export default {
  props: {
    value: String
  }
}
</script>

<style scoped>
.avatar {
  position: relative;
}
.avatar:hover .delete {
  display: block;
}
</style>
