<template>
  <b-card class="shadow">
    <b-card-title>
      <b-row class="justify-content-between px-2">
        <div>Id: {{ entity.id }}</div>
        <b-icon
          class="cursor-pointer"
          icon="pencil-square"
          @click="$emit('edit', entity)"
        ></b-icon>
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
      <div class="d-flex justify-content-between align-items-center mt-3">
        <b-icon
          @click="$emit('delete', entity)"
          class="cursor-pointer"
          scale="1.5"
          variant="danger"
          icon="trash"
        ></b-icon>
        <div
          class="text-left font-weight-bold"
          :class="entity.active ? 'text-success' : 'text-danger'"
        >
          {{ entity.active ? 'Active' : 'Inactive' }}
        </div>
      </div>
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
