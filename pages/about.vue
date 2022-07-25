<template>
  <div id="about">
    <div class="container">
      <div class="row justify-content-center">
        <div class="col-md-3">
          <!-- eslint-disable-next-line vue/no-v-html -->
          <div v-html="$md.render(about.title)" />

          <!-- eslint-disable-next-line vue/no-v-html -->
          <div v-html="$md.render(about.description)" />

          <Link :href="'https://wa.me/5561999561012'">
            Whatsapp
          </Link>
          <Link :href="'mailto:fabiovaz@gmail.com'">
            Email
          </Link>
          <Link :href="'https://www.linkedin.com/in/fabio-vaz-95a19631/'">
            LinkedIn
          </Link>
        </div>
      </div>
    </div>
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

<style lang="scss">
#about {
  z-index: 5;
  position: relative;
  padding: 160px 0 60px 0;
  text-align: center;
  h1, h2, h3, h4 {
    margin: 0;
  }
  h1 {
    font-family: 'Trump Gothic East', sans-serif;
    font-size: 88px;
    line-height: 68px;
  }
  h2 {
    font-family: 'Trump Gothic East', sans-serif;
    font-size: 32px;
    line-height: 32px;
  }
  h3 {
    font-size: 22px;
    line-height: 22px;
  }
  h4 {
    font-size: 16px;
    line-height: 22px;
  }
}
</style>
