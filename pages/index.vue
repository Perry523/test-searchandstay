<template>
  <b-container>
    <div class="d-flex justify-content-end mb-3 mt-5">
      <b-button pill variant="outline-primary" @click="entityModal = true">
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
          @edit="editEntity"
          @delete="handleDelete"
          @show="handleShow"
        />
      </div>
    </div>
    <b-modal v-model="entityModal">
      <template #modal-title>
        <h5 v-if="isShow">Entity {{ newEntity.id }}</h5>
        <h5 v-else>
          {{ isEdit ? 'Edit' : 'Create' }} entity {{ newEntity.id || '' }}
        </h5>
      </template>
      <div>
        <b-form-group label="Background Color">
          <b-input-group>
            <b-form-input
              v-model="newEntity.bg_color"
              :disabled="isShow"
              placeholder="#ffffff"
            />
            <template #append>
              <b-input-group-text style="width: 50px">
                <color-box :color="newEntity.bg_color" only-color />
              </b-input-group-text>
            </template>
          </b-input-group>
        </b-form-group>
        <b-form-group label="Text color">
          <b-input-group>
            <b-form-input
              v-model="newEntity.text_color"
              :disabled="isShow"
              placeholder="#000000"
            ></b-form-input>
            <template #append>
              <b-input-group-text style="width: 50px">
                <color-box :color="newEntity.text_color" only-color />
              </b-input-group-text>
            </template>
          </b-input-group>
        </b-form-group>
        <b-form-checkbox
          v-model="newEntity.active"
          :disabled="isShow"
          name="check-button"
          switch
        >
          Is active
        </b-form-checkbox>
      </div>
      <template #modal-footer>
        <b-button variant="secondary" @click="entityModal = false">
          {{ isShow ? 'Close' : 'Cancel' }}
        </b-button>
        <b-button v-if="!isShow" variant="primary" @click="saveEntity">
          Create
        </b-button>
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
        <b-button variant="primary" @click="deleteEntity"> Delete </b-button>
      </template>
    </b-modal>
  </b-container>
</template>

<script>
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
      entityModal: false,
      isEdit: false,
      isShow: false,
      confirmDelete: false,
    }
  },
  watch: {
    entityModal(val) {
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
        this.$toast.error(error.response.data.errors.message)
        // this.entities = payload.data.entities
      }
    },
    editEntity(entity) {
      this.newEntity = Object.assign({}, entity)
      this.newEntity.active = this.newEntity.active ? 1 : 0
      this.isEdit = true
      this.entityModal = true
    },
    async saveEntity() {
      const payload = {
        calendar_patterns: this.newEntity,
      }
      this.newEntity.active = this.newEntity.active ? 1 : 0
      if (this.isEdit) {
        await this.$axios.$put(
          `/calendar_patterns/${this.newEntity.id}`,
          payload
        )
      } else {
        await this.$axios.$post('/calendar_patterns', payload)
      }
      this.entityModal = false
      this.getEntities()
    },
    async handleShow(entity) {
      const { data } = await this.$axios.$get(`/calendar_patterns/${entity.id}`)
      this.newEntity = Object.assign({}, data)
      this.newEntity.active = !!entity.active
      this.isShow = true
      this.entityModal = true
    },
    async deleteEntity() {
      await this.$axios.$delete(`/calendar_patterns/${this.newEntity.id}`)
      this.confirmDelete = false
      this.getEntities()
    },
    handleDelete(entity) {
      this.newEntity = Object.assign({}, entity)
      this.confirmDelete = true
    },
    clearModal() {
      this.newEntity = {
        bg_color: '',
        text_color: '',
      }
      this.isEdit = false
      this.isShow = false
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
