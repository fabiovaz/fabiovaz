<template>
  <div>
    <header>
      <div class="logo">
        <NuxtLink to="/">
          <svg width="35" height="30" viewBox="0 0 35 30" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path class="f" d="M14.3588 3.85953H23.3434L25.5636 0H0L2.22021 3.85953H6.66064H14.3588Z" />
            <path class="v" d="M29.9833 0L27.7838 3.85953L17.2222 22.2601L11.0803 11.5786H13.8608H14.4625H18.9029L21.1232 7.71905H8.86011H7.05489H4.44043L17.2222 30L34.4444 0H29.9833Z" />
          </svg>
        </NuxtLink>
      </div>
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
      const key = e.key
      if (key === 'Escape' || key === 'Esc' || key === 27) {
        this.navOpen = false
      }
    })
  }
}
</script>

<style lang="scss">
header {
  z-index: 10;
  position: fixed;
  width: 100%;
}
.logo {
  position: fixed;
  top: 60px;
  left: 60px;
  .f {
    fill: #777;
  }
  .v {
    fill: #999;
  }
}
nav {
  z-index: 5;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  a {
    position: relative;
    padding: 10px 20px;
    font-family: 'Trump Gothic East', sans-serif;
    font-size: 58px;
    color: #eee;
    opacity: .5;
    overflow: hidden;
    text-transform: uppercase;
    text-decoration: none;
    transition: all 1s;
  }
}

.hamburguer {
  z-index: 15;
  position: absolute;
  display: block;
  width: 30px;
  height: 22px;
  top: 60px;
  right: 60px;
  .bar {
    display: block;
    background: #909090;
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
  &:hover {
    cursor: pointer;
    .bar {
      &:nth-child(1), &:nth-child(2), &:nth-child(3) {
        width: 30px;
        margin-left: 0;
      }
    }
  }
}
</style>
