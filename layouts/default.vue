<template>
  <div>
    <div id="error-effect" :class="{ 'dark' : !slider.state }" />
    <div id="error-glitch" />
    <header>
      <div class="logo">
        <NuxtLink to="/">
          <svg width="35" height="30" viewBox="0 0 35 30" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path class="f" d="M14.3588 3.85953H23.3434L25.5636 0H0L2.22021 3.85953H6.66064H14.3588Z" />
            <path class="v" d="M29.9833 0L27.7838 3.85953L17.2222 22.2601L11.0803 11.5786H13.8608H14.4625H18.9029L21.1232 7.71905H8.86011H7.05489H4.44043L17.2222 30L34.4444 0H29.9833Z" />
          </svg>
        </NuxtLink>
      </div>
      <div class="hamburguer" :class="{ 'open' : navOpen }" @click="navOpen = !navOpen; slider.state = !slider.state">
        <span class="bar" />
        <span class="bar" />
        <span class="bar" />
      </div>
    </header>
    <nav v-if="navOpen">
      <NuxtLink to="/">
        Home
        <span class="slice slice__top"><span>Home</span></span>
        <span class="slice slice__bottom"><span>Home</span></span>
      </NuxtLink>
      <NuxtLink to="/about">
        About
        <span class="slice slice__top"><span>About</span></span>
        <span class="slice slice__bottom"><span>About</span></span>
      </NuxtLink>
      <NuxtLink to="/works">
        Works
        <span class="slice slice__top"><span>Works</span></span>
        <span class="slice slice__bottom"><span>Works</span></span>
      </NuxtLink>
    </nav>

    {{ slider.state }}
    <Slider :jobs="slider.jobs" :state="slider.state" @changeState="slider.state = $event" />

    <Nuxt />
  </div>
</template>

<script>
export default {
  data () {
    return {
      slider: {
        jobs: [],
        state: false
      },
      navOpen: false
    }
  },
  async fetch () {
    this.slider.jobs = await this.$axios.$get('/jobs')
  },
  watch: {
    '$route.path' (v) {
      if (v === '/') {
        this.slider.state = true
      }
    },
    '$route' () {
      this.navOpen = false
    },
    'navOpen' (v) {
      const anime = this.$anime
      if (v) {
        this.$nextTick(() => {
          anime.remove('nav')
          anime({
            targets: 'nav',
            duration: 300,
            easing: 'easeOutQuad',
            scale: [1.2, 1],
            opacity: [0, 1]
          })
        })
      }
    }
  },
  mounted () {
    this.$nextTick(() => {
      this.slider.state = false
      if (this.$route.path === '/') {
        this.slider.state = true
      }
    })
    document.addEventListener('keydown', (e) => {
      if (e.defaultPrevented) {
        return
      }
      const key = e.key
      if (key === 'Escape' || key === 'Esc' || key === 27) {
        this.navOpen = false
      }
    })
  },
  methods: {
    // changeSliderState (value) {
    //   this.slider.state = value
    // }
  }
}
</script>

<style lang="scss">
body {
  background: #222;
  color: #eee;
  font-family: 'Inconsolata';
}
#error-effect {
  z-index: 1;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  &:before {
    content: '';
    position: absolute;
    width: 100%;
    height: 20%;
    background: rgba(255, 255, 255, 0.2);
    animation: noiseeffect 4000ms infinite linear;
    @keyframes noiseeffect {
      0% { top: -20%; opacity: 0; }
      20% { opacity: 0; }
      50% { opacity: 0.8; }
      80% { opacity: 0; }
      100% { top: 100%; opacity: 0; }
    }
  }
  &:after {
    content: '';
    position: fixed;
    width: 100%;
    height: 100%;
    background: radial-gradient(rgba(0, 0, 0, 0) 50%, rgba(0, 0, 0, 0.8)), linear-gradient(rgba(255, 255, 255, 0.4) 10%, transparent);
  }
  &.dark {
    &:after {
      background: radial-gradient(rgba(0, 0, 0, 0) 50%, rgba(0, 0, 0, 0.8)), linear-gradient(rgba(0, 0, 0, 0.1) 10%, transparent);
    }
  }
}
#error-glitch {
  z-index: 2;
  position: fixed;
  width: 100%;
  height: 100%;
  &:after {
    content: '';
    position: fixed;
    width: 100%;
    height: 100%;
    background:url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAAECAYAAABP2FU6AAAAE0lEQVQYV2NgYGD4zwAjQAwGBgAa+QH/eULVWwAAAABJRU5ErkJggg==') repeat;
    opacity: .4;
    animation: crt-lines 400ms normal infinite linear;
    @keyframes crt-lines {
        0% { transform: translateY(0px); }
        100% { transform: translateY(-4px); }
    }
  }
}
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
  position: fixed;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  a {
    display: block;
    position: relative;
    color: transparent;
    cursor: pointer;
    font-family: 'Trump Gothic East', sans-serif;
    font-size: 100px;
    letter-spacing: 8px;
    text-transform: uppercase;
    line-height: 1.2;
    &::before {
      z-index: 1;
      content: '';
      display: block;
      position: absolute;
      top: 50%;
      left: -10%;
      right: -10%;
      height: 4px;
      border-radius: 4px;
      margin-top: -2px;
      background: #eee;
      transform: scale(0);
      transition: transform .8s cubic-bezier(.16,1.08,.38,.98);
    }
    .slice {
      display: block;
      position: absolute;
      overflow: hidden;
      color: #9d9d9d;
      transition: all .8s cubic-bezier(.16,1.08,.38,.98);
      span { display: block; }
      &__top {
        top: 0;
        height: 50%;
      }
      &__bottom {
        top: 49.9%;
        height: 50.1%;
        span { transform: translateY(-50%); }
      }
    }

    &:hover {
      color: transparent;
      .slice {
        color: #FFF;
        &__top {
          transform: skewX(12deg) translateX(5px);
        }
        &__bottom {
          transform: skewX(12deg) translateX(-5px);
        }
      }
      &::before { transform: scale(1); }
    }
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
  &.open {
    .bar {
      width: 32px; margin-left: 0;
      &:nth-child(1) {
        transform: rotate(135deg) translateY(-10px) translateX(4px);
      }
      &:nth-child(2) {
        opacity: 0; transform: translateX(-20px);
      }
      &:nth-child(3) {
        transform: rotate(-135deg) translateY(10px) translateX(4px);
      }
    }
  }
}
</style>
