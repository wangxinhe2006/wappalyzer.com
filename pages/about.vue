<template>
  <div>
    <Page :title="title" hero narrow>
      <p>
        Wappalyzer started in 2008 as a
        <nuxt-link to="/download">browser extension</nuxt-link> for Mozilla
        Firefox. Since then, the project has become platform independent and
        grown into a successful business built around an
        <a href="https://github.com/aliasio/wappalyzer" target="blank"
          >open source</a
        >
        core. As a company, we're highly specialised in technographic data,
        continuously seeking ways to expand and improve our offering.
      </p>

      <p>
        The company was founded in Melbourne, Australia by
        <a href="https://alias.io" target="_blank">Elbert Alias</a> (<a
          href="https://www.linkedin.com/in/elbertalias/"
          target="_blank"
          ><v-icon color="accent" small>mdi-linkedin</v-icon></a
        >), a Dutch expatriate software engineer.
      </p>

      <h3 class="mt-8 mb-4">Get in touch</h3>

      <p>
        We love to hear from you. If you have any comments, questions or
        suggestions, please don't hesitate to
        <nuxt-link to="/contact">contact us</nuxt-link>.
      </p>
    </Page>
  </div>
</template>

<script>
import Page from '~/components/Page.vue'

export default {
  components: {
    Page
  },
  data() {
    return {
      title: 'Our story'
    }
  }
}
</script>
