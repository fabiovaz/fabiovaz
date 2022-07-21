<template>
  <div>
    <div id="jobs">
      <div v-for="job in jobs" :key="job.slug" class="job">
        <div class="background">
          <img :src="job.cover.url">
          <img :src="job.cover.url">
        </div>
      </div>
    </div>
    <div id="paginator">
      <div v-for="(job, index) in jobs" :key="job.id" class="item" :class="{ active: (slider.current) == index}" @click="change(index)">
        <span>{{ job.title }}</span>
        <a href="#">0{{ index + 1 }}</a>
      </div>
    </div>
    <div id="caption">
      <div class="number">
        0{{ slider.caption.index + 1 }}
      </div>
      <div v-if="slider.caption.title" class="title">
        <div v-lettering class="text">
          {{ slider.caption.title }}
        </div>
        <div v-if="slider.caption.title" class="shadow">
          <div v-lettering class="text">
            {{ slider.caption.title }}
          </div>
        </div>
      </div>
      <div class="description">
        <div class="mask" />
        <p>{{ slider.caption.subtitle }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  async asyncData ({ $axios, error }) {
    try {
      const { data } = await $axios.get('/jobs')
      return { jobs: data }
    } catch (e) {
      error(e)
    }
  },
  data () {
    return {
      jobs: [],
      slider: {
        total: 0,
        current: 0,
        timer: null,
        timeout: null,
        caption: {
          index: null,
          title: null,
          subtitle: null,
          slug: null
        }
      }
    }
  },
  mounted () {
    this.$nextTick(() => {
      this.slider.total = this.jobs.length - 1
      this.enter(0)
    })
  },
  methods: {
    teste () {
      clearInterval(this.slider.timer)
    },
    next () {
      const slide = this.slider.current < this.slider.total ? this.slider.current + 1 : 0
      this.change(slide)
    },
    prev () {
      const slide = this.slider.current === 1 ? this.slider.total : this.slider.current - 1
      this.change(slide)
    },
    change (value) {
      clearInterval(this.slider.timer)
      clearTimeout(this.slider.timeout)
      this.exit(this.slider.current)
      this.slider.current = value
      this.slider.timeout = setTimeout(() => {
        this.enter(this.slider.current)
      }, 900)
    },
    enter (index) {
      this.slider.caption = { ...this.slider.caption, index, title: this.jobs[index].title, subtitle: this.jobs[index].subtitle, slug: this.jobs[index].slug }
      this.$nextTick(() => {
        const anime = this.$anime
        anime({
          targets: `#jobs .job:nth-child(${index + 1})`,
          duration: 1000,
          easing: 'easeOutExpo',
          opacity: [0, 1],
          scale: [2, 1]
        })
        anime({
          targets: '#caption .title > .text span',
          duration: 600,
          easing: 'easeInQuad',
          delay: anime.stagger(60),
          opacity: [0, 1]
        })
        anime({
          targets: '#caption .title .shadow > .text span',
          duration: 600,
          easing: 'easeInQuad',
          delay: anime.stagger(60),
          opacity: [0, 1]
        })
        anime.set('#caption .description p', {
          opacity: 0
        })
        anime({
          targets: '#caption .description .mask',
          duration: 800,
          easing: 'easeInExpo',
          left: ['-100%', '0%'],
          complete () {
            anime.set('#caption .description p', {
              opacity: 1
            })
            anime({
              targets: '#caption .description .mask',
              duration: 800,
              easing: 'easeOutExpo',
              left: ['0%', '100%']
            })
          }
        })
      })
      this.timer()
    },

    exit (index) {
      this.slider.caption = { ...this.slider.caption, index, title: this.jobs[index].title, subtitle: this.jobs[index].subtitle, slug: this.jobs[index].slug }
      this.$nextTick(() => {
        const anime = this.$anime
        anime({
          duration: 1000,
          easing: 'easeInQuart',
          targets: `#jobs .job:nth-child(${index + 1})`,
          opacity: 0,
          scale: 1.2
        })
        anime({
          targets: '#caption .title > .text span',
          duration: 400,
          easing: 'easeOutQuad',
          delay: anime.stagger(30),
          opacity: 0
        })
        anime({
          targets: '#caption .title .shadow > .text span',
          duration: 400,
          easing: 'easeOutQuad',
          delay: anime.stagger(30),
          opacity: 0
        })
        anime({
          duration: 600,
          easing: 'easeInExpo',
          targets: '#caption .description .mask',
          left: ['-100%', '0%'],
          complete () {
            anime.set('#caption .description p', {
              opacity: 0
            })
            anime({
              targets: '#caption .description .mask',
              duration: 600,
              easing: 'easeOutExpo',
              left: ['0%', '100%']
            })
          }
        })
      })
    },
    timer () {
      this.slider.timer = setInterval(this.next, 6000)
    }
  }
}
</script>

<style lang="scss">
#caption {
  z-index: 5;
  position: absolute;
  top: 50%;
  left: 60px;
  transform: translateY(-50%);
  .number {
    position: absolute;
    top: -140px;
    font-family: 'Trump Gothic East', sans-serif;
    color: #333;
    font-size: 250px;
    line-height: 250px;
    opacity: .2;
  }
  .title {
    position: relative;
    overflow: hidden;
    cursor: pointer;
    .text {
      font: 800 90px/90px 'Trump Gothic East', sans-serif;
      text-transform: uppercase;
      color: #eee;
      animation: textBlink 2s normal infinite;
      @keyframes textBlink {
        0% { opacity: 1; transform: translateX(2px); }
        30% { opacity: .7; }
        70% { opacity: .8; transform: translateX(-2px); }
        71% { opacity: .8; transform: translateX(0px); }
        75% { opacity: .8; transform: translateX(1px); }
        100% { opacity: 7; transform: translateX(0px); }
      }
    }
    .shadow {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 40px;
      overflow: hidden;
      animation: shadowDown 3s normal infinite;
      @keyframes shadowDown {
        0% { top: 0px; }
        10% { top: 20px; }
        20% { top: 30px; }
        40% { top: 35px; }
        60% { top: 50px; }
        70% { top: 20px; }
        100% { top: 10px; }
      }
      .text {
        position: absolute;
        left: 0px;
        top: 0px;
        z-index: -1;
        animation: shadowTextUp 3s normal infinite;
        @keyframes shadowTextUp {
          0% { top: 0px; }
          10% { top: -20px; opacity: .5; }
          20% { top: -30px; opacity: 1; }
          40% { top: -35px; }
          60% { top: -50px; opacity: .5; }
          70% { top: -20px; }
          100% { top: -10px; opacity: 1; }
        }
      }
    }
  }
  .description {
    position: relative;
    overflow: hidden;
    display: inline-block;
    .mask {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: #eee;
    }
    p {
      margin: 0;
      font-family: 'Trump Gothic East', sans-serif;
      color: #eee;
      font-size: 18px;
      letter-spacing: 1px;
      text-transform: uppercase;
    }
  }
}
#jobs {
  position: fixed;
  width: 100%;
  height: 100%;
  overflow: hidden;
  .job {
    opacity: 0;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    .background {
      img {
        position: absolute;
        top: 0;
        width: 100%;
        height: 100%;
        object-fit: cover;
        opacity: 1;
        filter: grayscale(1);
        &:nth-child(1) {
          animation: crt-image 20ms alternate infinite;
          @keyframes crt-image {
            0% { transform: translateY(-1px); }
            100% { transform: translateY(0px); }
          }
        }
        &:nth-child(2) {
          animation: crt-imageglitch 1600ms alternate infinite;
          opacity: 0;
          @keyframes crt-imageglitch {
            0% { transform: translate(-40px, -2px); opacity: .3; }
            8% {transform: translate(40px, 2px); }
            16% {transform: translate(0px, 0px); opacity: 0; }
            100% { transform: translate(0px, 0px); }
          }
        }
      }
    }
  }
}
#paginator {
  z-index: 5;
  position: absolute;
  right: 60px;
  bottom: 60px;
  .item {
    position: relative;
    font-size: 12px;
    a {
      position: relative;
      display: block;
      color: #FFF;
      opacity: .5;
      padding-left: 45px;
      text-decoration: none;
      transition: all .3s;
      &:after {
        content: '';
        position: absolute;
        top: 50%;
        right: 20px;
        width: 30px;
        height: 2px;
        background: #FFF;
        transform: translateY(-50%);
        transition: all .3s;
      }
    }
    span {
      opacity: 0;
      display: inline-block;
      position: absolute;
      top: 0;
      width: 160px;
      right: 100%;
      text-align: right;
      color: #eee;
      transform: translateX(5px);
      transition: all .3s;
    }
    &:hover {
      span {
        opacity: 1;
        transform: translateX(0);
      }
      a {
        opacity: 1;
        &:after {
          transform: translateY(-50%) scale(1.1);
        }
      }
    }
    &.active {
      a {
        opacity: 1;
        &:after {
          transform: translateY(-50%) scale(1.1);
        }
      }
    }
  }
}
</style>
