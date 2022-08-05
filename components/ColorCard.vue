<template>
  <b-card class="shadow">
    <b-card-title>
      <b-row class="justify-content-between px-2">
        <div>Id: {{ entity.id }}</div>
        <b-icon class="cursor-pointer" icon="pencil-square"></b-icon>
      </b-row>
    </b-card-title>
    <b-card-text>
      <b-row
        class="align-items-center justify-content-between separator-bottom pb-2 mb-2"
      >
        <div class="ellipsis">Background color:</div>
        <color-box :color="entity.bg_color" />
      </b-row>
      <b-row class="align-items-center justify-content-between">
        <div class="ellipsis">Text color:</div>
        <color-box :color="entity.text_color" />
      </b-row>
    </b-card-text>
  </b-card>
</template>

<script>
export default {
  props: {
    entity: {
      type: Object,
      required: true,
    },
  },
}
</script>

<style></style>
