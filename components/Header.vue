<template>
  <div>
    <header>
      <div class="hamburguer" @click="navOpen = !navOpen">
        <span class="bar" />
        <span class="bar" />
        <span class="bar" />
      </div>
    </header>
    <nav v-if="navOpen">
      <NuxtLink to="/">
        Home
      </NuxtLink>
      <NuxtLink to="/about">
        About
      </NuxtLink>
      <NuxtLink to="/works">
        Works
      </NuxtLink>
    </nav>
  </div>
</template>

<script>
export default {
  data () {
    return {
      navOpen: false
    }
  },
  watch: {
    '$route' () {
      this.navOpen = false
    }
  },
  mounted () {
    document.addEventListener('keydown', (e) => {
      if (e.defaultPrevented) {
        return
      }
      const key = e.key || e.keyCode
      if (key === 'Escape' || key === 'Esc' || key === 27) {
        this.navOpen = false
      }
    })
  }
}
</script>

<style lang="scss">
nav {
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: blue;
  a {
    padding: 30px 0;
  }
}

.hamburguer {
  z-index: 55;
  position: absolute;
  display: block;
  width: 30px;
  height: 22px;
  top: 60px;
  right: 60px;
  .bar {
    display: block;
    background: #929292;
    height: 2px;
    margin-bottom: 8px;
    transition: all 0.3s;
    &:nth-child(1) {
      width: 30px;
    }
    &:nth-child(2) {
      width: 20px;
      margin-left: 10px;
    }
    &:nth-child(3) {
      width: 27px;
      margin-left: 3px;
      margin-bottom: 0;
    }
  }
}
</style>
