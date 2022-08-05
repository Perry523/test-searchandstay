<template>
  <b-container> </b-container>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      entities: [],
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
        this.$toast.error(error.response.data.errors.message)
      }
    },
  },
}
</script>
