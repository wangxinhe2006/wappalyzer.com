<template>
  <div />
</template>

<script>
export default {
  fetch({ app, store, params }) {
    store.commit('datasets/setCategorySlug', params.category)
    store.commit('datasets/setTechnologySlug', params.slug)

    app.router.push('/datasets')
  },
  created() {
    this.$router.replace('/datasets')
  }
}
</script>
