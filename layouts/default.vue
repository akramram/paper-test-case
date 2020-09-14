<template>
  <v-app>
    <div>
      <v-content>
        <v-row class="no-gutters">
          <v-col cols="1">
            <sidebar />
          </v-col>
          <v-col cols="*">
            <v-container
              fluid
              style="background-color:#F4FAFD; min-height:100vh"
            >
              <navbar />
              <nuxt />
            </v-container>
          </v-col>
        </v-row>
      </v-content>
    </div>
  </v-app>
</template>
<script>
import navbar from '../components/navbar'
import sidebar from '../components/sidebar'

export default {
  components: {
    navbar,
    sidebar
  }
}
</script>
