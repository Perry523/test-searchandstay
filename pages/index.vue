<template>
  <b-container>
    <div class="d-flex justify-content-end mb-3 mt-5">
      <b-button pill variant="outline-primary" @click="colorModal = true">
        <b-icon icon="plus"></b-icon>
        Add new
      </b-button>
    </div>
    <div class="row gx-3">
      <div
        v-for="(entity, i) in entities"
        :key="i"
        class="col-12 col-sm-6 col-lg-4 col-xl-3 mb-1 mb-sm-3"
      >
        <color-card
          :entity="entity"
          @edit="editColor"
          @delete="handleDelete"
        />
      </div>
    </div>
    <b-modal v-model="colorModal">
      <template #modal-title>
        <h5>
          {{ isEdit ? 'Edit' : 'Create' }} entity {{ newEntity.id || '' }}
        </h5>
      </template>
      <div>
        <b-form-group label="Background Color">
          <b-form-input v-model="newEntity.bg_color" placeholder="#ffffff" />
        </b-form-group>
        <b-form-group label="Text color">
          <b-form-input
            v-model="newEntity.text_color"
            placeholder="#000000"
          ></b-form-input>
        </b-form-group>
        <b-form-checkbox v-model="newEntity.active" name="check-button" switch>
          Is active
        </b-form-checkbox>
      </div>
      <template #modal-footer>
        <b-button variant="secondary" @click="colorModal = false">
          Cancel
        </b-button>
        <b-button variant="primary" @click="saveColor"> Create </b-button>
      </template>
    </b-modal>
    <b-modal v-model="confirmDelete">
      <template #modal-title>
        <h5>Confirm delete</h5>
      </template>
      <div>
        <p>Are you sure you want to delete entity {{ newEntity.id }}?</p>
      </div>
      <template #modal-footer>
        <b-button variant="secondary" @click="confirmDelete = false">
          Cancel
        </b-button>
        <b-button variant="primary" @click="deleteColor"> Delete </b-button>
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
        bg_color: '',
        text_color: '',
        active: 1,
      },
      colorModal: false,
      isEdit: false,
      confirmDelete: false, 
    }
  },
  watch: {
    colorModal(val) {
      if (!val) {
        this.clearModal()
      }
    },
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
      this.newEntity = Object.assign({}, entity)
      this.newEntity.active = !!entity.active
      this.isEdit = true
      this.colorModal = true
    },
    saveColor() {
      if (this.isEdit) {
        this.$axios.$put(
          `/calendar_patterns/${this.newEntity.id}`,
          this.newEntity
        )
      } else {
        this.$axios.$post('/calendar_patterns', this.newEntity)
      }
      this.colorModal = false
      this.getEntities()
    },
    deleteColor() {
      this.$axios.$delete(`/calendar_patterns/${this.newEntity.id}`)
      this.confirmDelete = false
      this.getEntities()
    },
    handleDelete(entity) {
      this.newEntity = Object.assign({}, entity)
      console.log('a')
      this.confirmDelete = true
    },
    clearModal() {
      this.newEntity = {
        bg_color: '',
        text_color: '',
      }
      this.isEdit = false
    },
  },
}
</script>
<style>
.separator-bottom {
  border-bottom: 1px solid #ebebeb;
}
.cursor-pointer {
  cursor: pointer;
}
</style>
