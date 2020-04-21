<template>
  <div>
    <Page :title="title" crumbs narrow>
      <p>
        The Wappalyzer Privacy Policy describes how we treat personal
        information when you use Wappalyzer's products and services, including
        information provided when you use the Wappalyzer browser extension
        ('extension'). In addition, the following describes our privacy
        practices that are specific to the extension.
      </p>

      <h2 id="information-we-collect" class="mt-8 mb-2">
        Information we collect
        <a href="#information-we-collect"
          ><v-icon color="accent">mdi-link</v-icon></a
        >
      </h2>

      <p>
        You do not need to provide any personal information in order to download
        and use the extension. All of the features can be used without sharing
        any personally identifiable information.
      </p>

      <p>
        The extension has some basic features that work automatically when you
        use the browser extension. Other features are optional and only operate
        and transmit data if you choose to have them enabled.
      </p>

      <h2 id="basic-features" class="mt-8 mb-2">
        Basic features
        <a href="#basic-features"><v-icon color="accent">mdi-link</v-icon></a>
      </h2>

      <p>
        Any time you use the extension to analyze a website, the extension sends
        basic, limited information including the domain name of the website you
        have visited and what application was identified on it. This data is
        retained in Wappalyzer's server logs and protected according to our
        general Privacy Policy but may be sold to or shared with our third party
        partners. At no time can this data be traced back to you personally,
        since no personally identifiable information is stored.
      </p>

      <h2 id="information-security" class="mt-8 mb-2">
        Information security
        <a href="#information-security"
          ><v-icon color="accent">mdi-link</v-icon></a
        >
      </h2>

      <p>
        Any information sent to Wappalyzer will be maintained in accordance with
        the Wappalyzer Privacy Policy. Wappalyzer's Privacy Policy does not
        cover information that you send to third party sites.
      </p>

      <h2 id="your-choices" class="mt-8 mb-2">
        Your choices
        <a href="#your-choices"><v-icon color="accent">mdi-link</v-icon></a>
      </h2>

      <p>
        Any feature that works by automatically sending information about sites
        when you visit them can be disabled and re-enabled in the extension
        settings.
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
      title: 'Privacy policy'
    }
  }
}
</script>
