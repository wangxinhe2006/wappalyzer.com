<template>
  <div>
    <Page :title="title" :head="meta" hero no-heading>
      <div class="mt-8">
        <v-btn color="accent" to="/faq/api" class="mb-2" outlined>
          <v-icon left>mdi-forum</v-icon>
          FAQs
        </v-btn>
        <v-btn
          color="accent"
          href="https://wappalyzer.com/docs/api"
          class="mb-2"
          outlined
        >
          <v-icon left>mdi-book-open-page-variant</v-icon>
          API reference
        </v-btn>
      </div>

      <template v-slot:content>
        <v-container class="apis pa-0 mt-12">
          <v-row class="apis__matrix pt-2 pr-1 pb-4" no-gutters>
            <v-col>
              <v-responsive height="245" />
              <template v-for="(attr, name) in attrs">
                <v-divider />
                <v-responsive height="60">
                  <v-card-text :key="name">
                    {{ attr }}
                  </v-card-text>
                </v-responsive>
              </template>
            </v-col>
            <v-col
              v-for="api in apis"
              :class="api.raised ? 'apis__col--raised' : ''"
              :key="api.name"
            >
              <v-responsive v-if="!api.raised" height="20"> </v-responsive>
              <v-card :raised="api.raised" class="text-center">
                <v-responsive v-if="!api.raised" height="10"> </v-responsive>
                <v-responsive v-if="api.raised" height="30">
                  <v-card-subtitle class="overline">
                    Most popular
                  </v-card-subtitle>
                </v-responsive>
                <v-responsive height="50">
                  <v-card-title class="justify-center">
                    {{ api.name }}
                  </v-card-title>
                </v-responsive>
                <v-responsive height="100">
                  <v-card-subtitle>
                    {{ api.description }}
                  </v-card-subtitle>
                </v-responsive>
                <v-responsive height="65">
                  <v-card-actions>
                    <v-btn
                      :to="api.route"
                      :text="!api.raised"
                      color="primary white-text"
                      class="mx-auto"
                      >Compare plans</v-btn
                    >
                  </v-card-actions>
                </v-responsive>
                <template v-for="(attr, name) in attrs">
                  <v-divider />
                  <v-responsive height="60">
                    <v-card-text :key="name" class="text-center">
                      <v-icon v-if="api.attrs[name] === true" color="success">
                        mdi-check
                      </v-icon>
                      <small v-else>
                        {{ api.attrs[name] }}
                      </small>
                    </v-card-text>
                  </v-responsive>
                </template>
                <v-responsive v-if="api.raised" height="20"> </v-responsive>
              </v-card>
            </v-col>
          </v-row>
        </v-container>

        <v-container>
          <small>* Subject to availability</small>
        </v-container>

        <v-row class="mb-4 mt-4 mt-sm-8">
          <v-col md="6">
            <h3>Example request (cURL)</h3>

            <v-card class="code mt-4" dark>
              <v-card-text>
                <pre class="code__block code__block--wrap">
<span class="code__key">curl</span> -H <span class="code__value">"x-api-key: wappalyzer.api.demo.key"</span> <span class="code__value">"https://api.wappalyzer.com/lookup/v1/?url=https://example.com"</span></pre>
              </v-card-text>
            </v-card>
          </v-col>
          <v-col md="6">
            <h3>Example response (JSON)</h3>

            <v-card class="code mt-4" dark>
              <v-card-text>
                <pre class="code__block">
[
  {
    <span class="code__key">"monthYear"</span>: <span class="code__value">"01-2020"</span>,
    <span class="code__key">"languages"</span>: [
      <span class="code__value">"en_US"</span>
    ],
    <span class="code__key">"applications"</span>: [
      {
        <span class="code__key">"name"</span>: <span class="code__value">"Craft CMS"</span>,
        <span class="code__key">"categories"</span>: [
          <span class="code__value">"CMS"</span>
        ],
        <span class="code__key">"versions"</span>: [
          <span class="code__value">"3.0.0"</span>
        ],
        <span class="code__key">"hits"</span>: <span class="code__value">5000</span>
      }
    ]
  }
]</pre>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </template>
    </Page>

    <Logos />
  </div>
</template>

<script>
import Page from '~/components/Page.vue'
import Logos from '~/components/Logos.vue'
import { attrs, apis } from '~/assets/json/apis.json'
import { apis as meta } from '~/assets/json/meta.json'

export default {
  components: {
    Page,
    Logos
  },
  data() {
    return {
      title: meta.title,
      attrs,
      apis,
      meta
    }
  }
}
</script>

<style>
.apis {
  overflow-x: auto;
}

.apis__matrix {
  min-width: 850px;
}

.apis__col--raised {
  z-index: 2;
}

.code {
  background-color: #282c34 !important;
}

.code__block {
  line-height: 1.1rem;
}

.code__block--wrap {
  white-space: normal;
}

.code__key {
  color: #f8c555;
}

.code__value {
  color: #7ec699;
}
</style>
