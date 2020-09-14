<template>
  <v-container>
    <v-card outlined class="mx-auto">
      <v-card-text>
        <form @submit.prevent="userLogin">
          <div>
            <label>Username</label>
            <v-text-field v-model="login.username" outlined type="text" />
          </div>
          <div>
            <label>Password</label>
            <v-text-field v-model="login.password" outlined type="text" />
          </div>
          <div>
            <v-btn color="primary" type="submit">Submit</v-btn>
          </div>
        </form>
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    login: {
      username: '',
      password: ''
    }
  }),
  methods: {
    async userLogin() {
      try {
        const response = await this.$auth.loginWith('local', {
          data: this.login
        })
        // eslint-disable-next-line no-console
        console.log(response)
        this.$store.commit('setUser', response.data)
        // this.getFinance()
        // this.getaccountlist()
      } catch (err) {
        // eslint-disable-next-line no-console
        console.log(err)
      }
    }
  }
}
</script>

<style></style>
