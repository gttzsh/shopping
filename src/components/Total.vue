<template>
  <div>
    总价:{{total}}元
  </div>
</template>
<script>
export default {
  props: {
    total:{
      type: Number,
      default() {
        return 0
      }
    }
  }
}
</script>
