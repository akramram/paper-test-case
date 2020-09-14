<template>
  <div>
    <v-app-bar flat color="transparent" class="text--primary">
      <div class="d-flex align-center mb-2">
        <v-btn
          inset
          depressed
          @click="$vuetify.theme.dark = !$vuetify.theme.dark"
          >Theme</v-btn
        >
      </div>
      <v-spacer></v-spacer>

      <v-btn icon to="login">
        <v-icon>person</v-icon>
      </v-btn>
      <div class="primary--text mr-2">
        {{ user ? user.name : '' }}
      </div>

      <v-btn
        v-if="$auth.loggedIn"
        color="primary"
        depressed
        @click="$auth.logout()"
        >Logout</v-btn
      >
    </v-app-bar>
  </div>
</template>
<script>
import { mapState } from 'vuex'
export default {
  computed: {
    ...mapState(['user'])
  }
}
</script>
