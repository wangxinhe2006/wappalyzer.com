<template>
  <div>
    <Page
      :title="title"
      :head="{ ...meta, image: 'extension-2.svg' }"
      hero
      no-heading
    >
      <div class="mt-8 mb-4">
        <v-btn
          v-for="{ text, href, icon } in platforms"
          :key="text"
          :href="href"
          color="primary"
          class="mr-2 mb-4"
          target="_blank"
          outlined
        >
          <v-icon left>{{ icon }}</v-icon>
          {{ text }}
        </v-btn>
      </div>

      <v-divider class="my-8" />

      <h3 class="mb-2">Bookmarklet</h3>

      <p class="mb-8">
        Using an unsupported web browser? Drag this button to your bookmarks
        toolbar to identify technologies with a click.
      </p>

      <v-btn
        href="javascript: (function() { var d = document, e = d.getElementById('wappalyzer-container') ; if ( e !== null ) { d.body.removeChild(e); } var u = 'https://www.wappalyzer.com/', t = new Date().getTime(), c = d.createElement('div'), p = d.createElement('div'), l = d.createElement('link'), s = d.createElement('script') ; c.setAttribute('id', 'wappalyzer-container'); l.setAttribute('rel', 'stylesheet'); l.setAttribute('href', u + 'css/bookmarklet.css'); d.head.appendChild(l); p.setAttribute('id', 'wappalyzer-pending'); p.setAttribute('style', 'background-image: url(' + u + 'images/spinner.gif) !important'); c.appendChild(p); s.setAttribute('src', u + 'bookmarklet/wappalyzer.js'); s.onload = function() { window.wappalyzer = new Wappalyzer(); s = d.createElement('script'); s.setAttribute('src', u + 'bookmarklet/apps.js'); s.onload = function() { s = d.createElement('script'); s.setAttribute('src', u + 'bookmarklet/driver.js'); c.appendChild(s); }; c.appendChild(s); }; c.appendChild(s); d.body.appendChild(c); })();"
        color="primary"
        class="mb-4"
        outlined
      >
        Wappalyzer
      </v-btn>

      <v-divider class="my-8" />

      <h3 class="mb-2">Contribute!</h3>

      <p class="mb-8">
        The browser extension is open source, which means that anyone can add
        new technologies or respond to issues.
      </p>

      <v-btn
        href="https://github.com/aliasio/wappalyzer"
        target="_blank"
        color="accent"
        class="mb-4"
        outlined
      >
        <v-icon left>mdi-github</v-icon>
        Contribute on GitHub
      </v-btn>
    </Page>
  </div>
</template>

<script>
import Page from '~/components/Page.vue'
import { download as meta } from '~/assets/json/meta.json'

export default {
  components: {
    Page
  },
  data() {
    return {
      title: meta.title,
      meta,
      platforms: [
        {
          text: 'Google Chrome',
          href:
            'https://chrome.google.com/webstore/detail/wappalyzer/gppongmhjkpfnbhagpmjfkannfbllamg',
          icon: 'mdi-google-chrome'
        },
        {
          text: 'Mozilla Firefox',
          href: 'https://addons.mozilla.org/en-US/firefox/addon/wappalyzer/',
          icon: 'mdi-firefox'
        },
        {
          text: 'Microsoft Edge',
          href:
            'https://microsoftedge.microsoft.com/addons/detail/mnbndgmknlpdjdnjfmfcdjoegcckoikn',
          icon: 'mdi-microsoft-edge'
        }
      ]
    }
  }
}
</script>
