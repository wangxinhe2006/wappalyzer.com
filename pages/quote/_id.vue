<template>
  <Page :title="title" :head="{ title, text }">
    <v-btn
      :href="`https://legacy.wappalyzer.com/quote/${id}`"
      class="mb-4"
      color="accent"
      target="_blank"
      outlined
    >
      View this page on our old website
      <v-icon right>mdi-open-in-new</v-icon>
    </v-btn>
  </Page>
</template>

<script>
import Page from '~/components/Page.vue'

export default {
  components: {
    Page
  },
  data() {
    return {
      title: 'Quote',
      text: `We've made some changes to our website, including the way quotes are managed. To view your quote on our old website, please use the link below.`,
      id: this.$route.params.id
    }
  }
}
</script>
