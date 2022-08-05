<template>
  <b-container>
    <div class="row gx-3">
      <div
        v-for="(entity, i) in entities"
        :key="i"
        class="col-12 col-sm-6 col-lg-4 col-xl-3"
      >
        <color-card @edit="editColor(entity)" :entity="entity" />
      </div>
    </div>
    <b-modal>
      <template v-slot:modal-title>
        <h5>Create new color</h5>
      </template>
      <template v-slot:modal-body>
        <b-form-group label="Background color">
          <color-box v-model="newEntity.bg_color" :color="newEntity.bg_color" />
        </b-form-group>
        <b-form-group label="Text color">
          <color-box
            v-model="newEntity.text_color"
            :color="newEntity.text_color"
          />
        </b-form-group>
      </template>
      <template v-slot:modal-footer>
        <b-button variant="secondary" @click="modal = false"> Cancel </b-button>
        <b-button variant="primary" @click="createColor"> Create </b-button>
      </template>
    </b-modal>
  </b-container>
</template>

<script>
const payload = {
  success: true,
  data: {
    pagination: {
      total: 2,
      count: 2,
      per_page: 10,
      current_page: 1,
      total_pages: 1,
      links: {
        next: null,
        prev: null,
      },
    },
    entities: [
      {
        id: 1,
        bg_color: '#ff0000',
        text_color: '#000000',
        active: 1,
      },
      {
        id: 2,
        bg_color: '#00ff00',
        text_color: '#000000',
        active: 1,
      },
    ],
  },
  message: 'Entities retrieved successfully.',
}
export default {
  name: 'IndexPage',
  data() {
    return {
      entities: [],
      newEntity: {
        bg_color: '#ff0000',
        text_color: '#000000',
      },
    }
  },
  created() {
    this.getEntities()
  },
  methods: {
    async getEntities() {
      try {
        const { data } = await this.$axios.$get('/calendar_patterns')
        this.entities = data.entities
      } catch (error) {
        // this.$toast.error(error.response.data.errors.message)
        this.entities = payload.data.entities
      }
    },
    editColor(entity) {
      this.newEntity = entity
      this.modal = true
    },
    createColor() {
      this.entities.push(this.newEntity)
      this.modal = false
    },
  },
}
</script>
<style>
.separator-bottom {
  border-bottom: 1px solid #ebebeb;
}
</style>
