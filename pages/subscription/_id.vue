<template>
  <Page :title="title" :head="{ title, text }" narrow>
    <v-btn
      :href="`https://legacy.wappalyzer.com/subscription/${id}`"
      class="mb-8"
      color="accent"
      target="_blank"
      outlined
    >
      View this page on our old website
      <v-icon right>mdi-open-in-new</v-icon>
    </v-btn>

    <v-card class="my-4">
      <v-card-title>
        Link subscription
      </v-card-title>
      <v-card-text>
        <v-alert v-if="success" type="success">
          {{ success }}
        </v-alert>
        <v-alert v-if="error" type="error">
          {{ error }}
        </v-alert>

        <p>
          Please enter the email address associated with your subscription.
        </p>

        <v-form ref="form" v-on:submit.prevent="submit">
          <v-text-field
            v-model="email"
            :rules="[(v) => !v || /@/.test(v) || 'Enter a valid email address']"
            label="Email address"
            required
          />
        </v-form>
      </v-card-text>
      <v-card-actions>
        <v-spacer />
        <v-btn @click="submit" :loading="submitting" color="accent" text>
          <v-icon left>mdi-link-variant</v-icon>
          Link subscription
        </v-btn>
      </v-card-actions>
    </v-card>

    <v-dialog v-model="signInDialog" max-width="400px">
      <SignIn mode-sign-up mode-continue />
    </v-dialog>
  </Page>
</template>

<script>
import Page from '~/components/Page.vue'
import SignIn from '~/components/SignIn.vue'

export default {
  components: {
    Page,
    SignIn
  },
  data() {
    return {
      title: 'Subscription',
      text: `We've made some changes to our website, including the way subscriptions are managed. Use the form below to link your subscription to an account. Alternatively, you can view this page on our old website.`,
      id: this.$route.params.id,
      email: '',
      error: false,
      signInDialog: false,
      submitting: false,
      success: false
    }
  },
  watch: {
    '$store.state.user.isSignedIn'(isSignedIn) {
      if (isSignedIn && this.signInDialog) {
        this.signInDialog = false

        this.submit()
      }
    }
  },
  methods: {
    async submit() {
      this.error = false
      this.success = false

      if (!this.$store.state.user.isSignedIn) {
        this.signInDialog = true

        return
      }

      this.submitting = true

      try {
        await this.$axios.post(`subscriptions/claim/${this.id}`, {
          email: this.email
        })

        this.email = ''
        this.success =
          'Success! Your subscription has been linked to your account.'
      } catch (error) {
        this.error =
          error.response && typeof error.response.data === 'string'
            ? error.response.data
            : this.getErrorMessage(error)
      }

      this.submitting = false
    }
  }
}
</script>
