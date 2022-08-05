<template>
  <div class="d-flex border container-size px-1">
    <div
      :style="`background: ${color}`"
      class="color-box border"
    ></div>
    {{ color }}
  </div>
</template>

<script>
export default {
  props: {
    color: {
      type: String,
      required: true,
    },
  },
}
</script>

<style>
.color-box {
  width: 20px;
  height: 20px;
  border-radius: 50%;
}
.container-size {
  min-width: 95px;
}
</style>
