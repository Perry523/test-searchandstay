<template>
  <div class="d-flex flex-column items-center justify-content-center vh-100">
    <b-form class="form-container" @submit.prevent="login">
      <nuxt-logo class="mb-5"></nuxt-logo>
      <b-form-group>
        <b-form-input
          v-model="user.email"
          required
          type="text"
          placeholder="Username"
        />
      </b-form-group>
      <b-form-group>
        <b-form-input
          v-model="user.password"
          required
          type="password"
          placeholder="Password"
        />
      </b-form-group>
      <b-button type="submit" variant="primary">Login</b-button>
    </b-form>
  </div>
</template>

<script>
export default {
  auth: 'guest',
  data() {
    return {
      user: {
        email: '',
        password: '',
      },
    }
  },
  methods: {
    login() {
      const { email, password } = this.user
      const payload = {
        login: {
          email,
          password,
        },
      }
      this.$auth
        .loginWith('local', { data: payload })
        .then(({ data }) => {
          this.$auth.setUser(data)
          this.$toast.success(data.message)
          this.$router.push('/')
        })
        .catch((error) => {
          this.$toast.show(error.response.data.data)
        })
    },
  },
}
</script>

<style>
.form-container {
  max-width: 400px;
  margin: 0 auto;
}
</style>
