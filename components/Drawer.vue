<template>
  <v-navigation-drawer v-model="isOpen" right temporary app>
    <template v-if="user.email">
      <v-list nav dense>
        <v-list-item>
          <v-list-item-content>
            <v-list-item-title class="mb-2">
              Signed in as
            </v-list-item-title>

            <small
              >{{ user.email }}
              <v-btn
                v-if="user.admin"
                to="/admin"
                class="my-n2"
                color="success"
                icon
                small
              >
                <v-icon small>mdi-lock-open</v-icon>
              </v-btn>
            </small>
          </v-list-item-content>
        </v-list-item>
      </v-list>
      <v-divider />
    </template>
    <div v-for="(item, i) in mainNav" :key="i">
      <v-list nav dense>
        <v-subheader v-if="item.items">
          {{ item.title }}
        </v-subheader>
        <v-list-item v-else :to="item.to" color="primary" nuxt>
          <v-list-item-title>
            {{ item.title }}
          </v-list-item-title>
        </v-list-item>
        <v-list-item
          v-if="item.items"
          v-for="(subitem, j) in item.items"
          :key="j"
          :href="subitem.to.match(/^http/) ? subitem.to : null"
          :target="subitem.to.match(/^http/) ? '_blank' : '_self'"
          :to="subitem.to.match(/^http/) ? null : subitem.to"
          color="primary"
          nuxt
        >
          <v-list-item-title>
            {{ subitem.title }}
          </v-list-item-title>
        </v-list-item>
      </v-list>
      <v-divider />
    </div>
    <v-list nav dense>
      <v-list-item
        v-for="(item, i) in userNav"
        :key="i"
        :to="item.to"
        color="primary"
        nuxt
      >
        <v-list-item-title>
          {{ item.title }}
        </v-list-item-title>
        <v-list-item-icon v-if="item.icon">
          <v-icon dense>{{ item.icon }}</v-icon>
        </v-list-item-icon>
      </v-list-item>
    </v-list>
  </v-navigation-drawer>
</template>

<script>
export default {
  props: {
    mainNav: {
      type: Array,
      default: () => []
    },
    userNav: {
      type: Array,
      default: () => []
    },
    user: {
      type: Object,
      default() {
        return {}
      }
    }
  },
  data() {
    return {
      isOpen: false
    }
  },
  methods: {
    open() {
      this.isOpen = true
    }
  }
}
</script>
