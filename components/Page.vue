<template>
  <div>
    <Crumbs v-if="!noCrumbs" :crumbs="crumbNav" />

    <v-container class="py-10 py-sm-12">
      <v-row v-if="secure && !isSignedIn" no-gutters>
        <SignIn mode-continue />
      </v-row>
      <v-row v-else-if="sideNav.length">
        <v-col cols="12" sm="4" lg="3" order="2" order-sm="0">
          <SideNav :items="sideNav" />
        </v-col>
        <v-col cols="12" sm="8" lg="9">
          <PageHead
            v-if="!noHead"
            :loading="loading"
            :title="head.title"
            :subtitle="head.subtitle"
            :text="head.text"
            :image="head.image"
          >
            <slot />
          </PageHead>
          <slot v-else />
          <slot name="content" />
        </v-col>
      </v-row>
      <v-row v-else-if="narrow" no-gutters>
        <v-col sm="10" md="10" lg="8">
          <PageHead
            v-if="!noHead"
            :loading="loading"
            :title="head.title"
            :subtitle="head.subtitle"
            :text="head.text"
            :image="head.image"
          >
            <slot />
          </PageHead>
          <slot v-else />
          <slot name="content" />
        </v-col>
      </v-row>
      <template v-else>
        <PageHead
          v-if="!noHead"
          :loading="loading"
          :title="head.title"
          :subtitle="head.subtitle"
          :text="head.text"
          :image="head.image"
        >
          <slot />
        </PageHead>
        <slot v-else />
        <slot name="content" />
      </template>
    </v-container>
  </div>
</template>

<script>
import { mapState, mapActions } from 'vuex'

import PageHead from '~/components/PageHead.vue'
import SideNav from '~/components/SideNav.vue'
import Crumbs from '~/components/Crumbs.vue'
import SignIn from '~/components/SignIn.vue'
import userNav from '~/assets/json/nav/user.json'

export default {
  components: {
    PageHead,
    SideNav,
    Crumbs,
    SignIn
  },
  head() {
    return {
      title: this.seoTitle || this.title,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: this.head.meta || this.head.text || ''
        }
      ]
    }
  },
  props: {
    crumbs: {
      type: Array,
      default: () => []
    },
    side: {
      type: Array,
      default: () => []
    },
    seoTitle: {
      type: String,
      default: ''
    },
    title: {
      type: String,
      default: ''
    },
    noCrumbs: {
      type: Boolean,
      default: false
    },
    head: {
      type: Object,
      default() {
        return {
          loading: this.loading,
          title: this.title,
          subtitle: '',
          text: '',
          image: '',
          meta: ''
        }
      }
    },
    noHead: {
      type: Boolean,
      default: false
    },
    narrow: {
      type: Boolean,
      default: false
    },
    hero: {
      type: Boolean,
      default: false
    },
    secure: {
      type: Boolean,
      default: false
    },
    loading: {
      type: Boolean,
      default: false
    }
  },
  computed: {
    ...mapState({
      isSignedIn: ({ user }) => user.isSignedIn
    }),
    sideNav() {
      return this.secure
        ? this.isSignedIn
          ? userNav
          : userNav.filter((item) => !item.auth)
        : this.side
    },
    crumbNav() {
      return [...this.crumbs, { title: this.title, to: '' }]
    }
  },
  created() {
    this.set({
      hero: this.hero,
      secure: this.secure
    })
  },
  methods: {
    ...mapActions({
      set: 'page/set'
    })
  }
}
</script>
