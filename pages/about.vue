<template>
  <div class="caption">
    <client-only>
      <h1 class="anime">
        NUXT ANIMEJS
      </h1>
    </client-only>
    {{ about.title }}
    {{ about.description }}
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
  },
  mounted () {
    this.startAnim()
  },
  methods: {
    startAnim () {
      this.$anime({
        easing: 'easeOutExpo',
        targets: '.anime',
        opacity: 0,
        scale: 1.2
      })
    }
  }
}
</script>

<style lang="scss">
.caption {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
</style>
