<template>
  <div />
</template>

<script>
export default {
  fetch({ app, store, params }) {
    store.commit('datasets/setCategorySlug', params.category)

    app.router.push('/datasets')
  },
  created() {
    this.$router.replace('/datasets')
  }
}
</script>
