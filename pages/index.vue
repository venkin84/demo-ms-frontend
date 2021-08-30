<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card class="logo py-8 d-flex justify-center">
        <NuxtLogo />
        <VuetifyLogo class="ml-4" />
      </v-card>
      <v-card>
        <v-card-title class="headline text-body-1 word-wrap">
          Innso Selfservice Client Platform
        </v-card-title>
        <v-card-text>
          <p>
            Welcome to the Innso Client Service! This client platform is built
            using Vuetify + Nuxt.js.
          </p>
          <p>
            This reusable platform can do SSG for client specific deployments.
            Also, it can be hosted as a multi tenant platform to host multiple
            client services in single instance
          </p>
        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn class="mb-4" color="primary" nuxt to="/contact-us">
            Continue
          </v-btn>
          <v-spacer />
        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>
</template>
<script>
export default {
  layout: 'innsoTheme',
  head() {
    return {
      title: 'Innso Client Service - Welcome',
    }
  },
}
</script>
<style scoped>
.word-wrap {
  word-break: break-word;
}
</style>
