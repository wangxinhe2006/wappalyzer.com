<template>
  <div>
    <Page
      :title="title"
      :head="{ title: title, subtitle: api.description, text: meta.text }"
      :crumbs="[{ title: 'APIs', to: '/api' }]"
      hero
    >
      <div class="mt-4">
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

      <div
        v-if="$route.params.id !== 'dataset'"
        class="d-flex justify-center my-12"
      >
        <v-btn-toggle v-model="interval" color="primary" class="mr-4" mandatory>
          <v-btn value="month" class="overline px-8" outlined>
            Monthly
          </v-btn>
          <v-btn value="year" class="overline px-8" outlined>
            Annually
          </v-btn>
        </v-btn-toggle>
      </div>

      <v-row class="plans__matrix mt-12" no-gutters>
        <v-col>
          <v-responsive :height="Object.keys(plans).length > 1 ? 155 : 125" />
          <template v-for="attr in planAttrs">
            <v-divider />
            <v-responsive :key="attr.text" height="60">
              <v-card-text>
                {{ attr.text }}
              </v-card-text>
            </v-responsive>
          </template>
        </v-col>
        <v-col
          v-for="(plan, id) in plans"
          :class="plan.raised ? 'plans__col--raised' : ''"
          :key="plan.size"
        >
          <v-responsive v-if="!plan.raised" height="20"> </v-responsive>
          <v-card :raised="plan.raised" class="text-center">
            <v-responsive v-if="!plan.raised" height="10"> </v-responsive>
            <v-responsive
              v-if="plan.raised && Object.keys(plans).length > 1"
              height="30"
            >
              <v-card-subtitle class="overline">
                Most popular
              </v-card-subtitle>
            </v-responsive>
            <v-responsive height="60">
              <v-card-title class="justify-center">
                {{ plan.size }}
              </v-card-title>
            </v-responsive>
            <v-responsive height="65">
              <v-card-actions>
                <v-btn
                  @click="subscribe(id)"
                  :text="!plan.raised"
                  color="primary white-text"
                  class="mx-auto"
                  >Order</v-btn
                >
              </v-card-actions>
            </v-responsive>
            <template v-for="(attr, name) in planAttrs">
              <v-divider />
              <v-responsive height="60">
                <v-card-text :key="name" class="text-center">
                  <template v-if="attr.type === 'currency'">
                    <template v-if="plan.attrs[name]">
                      <span class="font-weight-medium">
                        {{ formatCurrency(plan.attrs[name] / 100) }}
                      </span>
                      <template v-if="name === 'amount'">
                        / {{ plan.interval }}
                      </template>
                    </template>
                    <template v-else>
                      Free
                    </template>
                  </template>
                  <template v-else-if="attr.type === 'number'">
                    <template v-if="plan.attrs[name]">
                      {{ formatNumber(plan.attrs[name]) }}
                    </template>
                    <template v-else>
                      Unlimited
                    </template>
                  </template>
                  <template v-else>
                    {{ plan.attrs[name] }}
                  </template>
                </v-card-text>
              </v-responsive>
            </template>
            <v-responsive v-if="plan.raised" height="20"> </v-responsive>
          </v-card>
        </v-col>
      </v-row>

      <v-container>
        <small>
          Prices are in Australian dollars.<br />
          <nuxt-link to="/contact">Contact us</nuxt-link> for firehose access
          and tailored plans.<br />
          By using our services, you agree to our
          <nuxt-link to="/terms">terms of service</nuxt-link>.
        </small>
      </v-container>

      <div class="py-2" />

      <v-card class="mt-12 mb-6">
        <v-card-title>
          Compare APIs
        </v-card-title>
        <v-card-text class="pa-0">
          <v-simple-table>
            <thead>
              <tr>
                <th width="20%"></th>
                <th
                  v-for="_api in apis"
                  :key="_api.name"
                  :class="
                    `text-center ${
                      _api.name === api.name ? 'apis__col--highlight' : ''
                    }`
                  "
                  width="20%"
                >
                  <v-btn :to="_api.route" color="accent" small text>{{
                    _api.name
                  }}</v-btn>
                </th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(attr, name) in attrs">
                <td width="20%" class="caption">{{ attr }}</td>
                <td
                  v-for="_api in apis"
                  :key="_api.name"
                  :class="
                    `text-center ${
                      _api.name === api.name ? 'apis__col--highlight' : ''
                    }`
                  "
                >
                  <v-icon v-if="_api.attrs[name] === true" color="success">
                    mdi-check
                  </v-icon>
                  <small v-else>
                    {{ _api.attrs[name] }}
                  </small>
                </td>
              </tr>
            </tbody>
          </v-simple-table>
        </v-card-text>
      </v-card>

      <small>* Subject to availability</small>

      <v-dialog v-model="signInDialog" max-width="400px">
        <SignIn mode-continue mode-sign-up />
      </v-dialog>

      <OrderDialog
        ref="orderDialog"
        v-on:close="orderDialog = false"
        :error="orderError"
        :id="order ? order.id : null"
      />
    </Page>

    <Logos />
  </div>
</template>

<script>
import Page from '~/components/Page.vue'
import Logos from '~/components/Logos.vue'
import SignIn from '~/components/SignIn.vue'
import OrderDialog from '~/components/OrderDialog.vue'
import { apis as meta } from '~/assets/json/meta.json'
import { attrs, planAttrs, apis } from '~/assets/json/apis.json'

export default {
  components: {
    Page,
    Logos,
    SignIn,
    OrderDialog
  },
  data() {
    return {
      title: `${apis[this.$route.params.id].name} API`,
      api: apis[this.$route.params.id],
      apis,
      attrs,
      order: false,
      orderError: '',
      interval: this.$route.params.id === 'dataset' ? 'year' : 'month',
      meta,
      planAttrs,
      signInDialog: false,
      subscribing: false
    }
  },
  computed: {
    plans() {
      return Object.keys(this.api.plans).reduce((plans, plan) => {
        if (this.api.plans[plan].interval === this.interval) {
          plans[plan] = this.api.plans[plan]
        }

        return plans
      }, {})
    }
  },
  watch: {
    '$store.state.user.isSignedIn'(isSignedIn) {
      if (isSignedIn && this.subscribing) {
        this.signInDialog = false

        this.subscribe(this.subscribing)
      }
    }
  },
  methods: {
    async subscribe(plan) {
      this.order = false
      this.orderError = ''
      this.subscribing = plan

      if (!this.$store.state.user.isSignedIn) {
        this.signInDialog = true

        return
      }

      this.$refs.orderDialog.open()

      try {
        this.order = (
          await this.$axios.put('orders', {
            product: 'API',
            plan
          })
        ).data
      } catch (error) {
        this.orderError = this.getErrorMessage(error)
      }
    }
  }
}
</script>

<style>
.plans__matrix {
  min-width: 850px;
}

.plans__col--raised {
  z-index: 2;
}

th.apis__col--highlight,
tr:not(:hover) td.apis__col--highlight {
  background-color: #fafafa;
}

th.apis__col--highlight {
}
</style>
