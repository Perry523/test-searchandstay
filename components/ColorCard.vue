<template>
  <b-card class="shadow">
    <b-card-title>
      <b-row class="justify-content-between pl-2">
        <div>Id: {{ entity.id }}</div>
        <b-dropdown
          dropright
          style="margin-top: -8px"
          variant="link-dark"
          class="text-decoration-none"
          toggle-class="text-decoration-none"
          no-caret
        >
          <template #button-content>
            <b-icon
              class="cursor-pointer cursor-decoration-none"
              icon="three-dots-vertical"
            ></b-icon>
          </template>
          <b-dropdown-item class="py-1" @click="$emit('show', entity)">
            <b-icon icon="search" class="mr-1" />Show details</b-dropdown-item
          >
          <b-dropdown-item class="py-1" @click="$emit('edit', entity)"
            ><b-icon icon="pencil-square" class="mr-1" />Edit</b-dropdown-item
          >
          <b-dropdown-item class="py-1" @click="$emit('delete', entity)"
            ><b-icon icon="trash" class="mr-1" />Remove</b-dropdown-item
          >
        </b-dropdown>
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
      <div
        class="text-center font-weight-bold mt-3"
        :class="entity.active ? 'text-success' : 'text-danger'"
      >
        {{ entity.active ? 'Active' : 'Inactive' }}
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
