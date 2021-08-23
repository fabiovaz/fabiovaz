<template>
  <div>
    About
  </div>
</template>

<script>
export default {
  async asyncData ({ $axios, error }) {
    try {
      const { data } = await $axios.get('/about')
      return { about: data }
    } catch (e) {
      error(e)
    }
  }
}
</script>
