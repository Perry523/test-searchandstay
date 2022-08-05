<template>
  <div class="d-flex container-size px-1" :class="!onlyColor ? 'border' : ''">
    <div :style="`background: ${color}`" class="color-box border"></div>
    <div v-if="!onlyColor">
      {{ color }}
    </div>
  </div>
</template>

<script>
export default {
  props: {
    color: {
      type: String,
      required: true,
    },
    onlyColor: {
      type: Boolean,
      default: false,
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
