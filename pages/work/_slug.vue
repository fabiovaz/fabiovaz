<template>
  <div id="work">
    <div class="container">
      <div class="row">
        <div class="col">
          <div class="title">
            <div class="caption">
              <h1>{{ job.title }}</h1>
              <span class="mask" />
            </div>
          </div>
          <div class="subtitle">
            <div class="caption">
              <h4>{{ job.subtitle }}</h4>
              <span class="mask" />
            </div>
            <div class="link">
              <Link :href="job.url">
                View Online
              </Link>
            </div>
          </div>
        </div>
      </div>
      <div class="row">
        <div class="col mt-4">
          {{ job.description }}
        </div>
      </div>
      <div class="row">
        <div class="col">
          <div class="gallery">
            <div v-for="(image, index) in job.gallery" :key="index" class="browser-mockup">
              <img :src="image.url">
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  async asyncData ({ params, $axios }) {
    const { data } = await $axios.get(`/jobs/?slug=${params.slug}`)
    return { job: data[0] }
  },
  head () {
    return {
      title: this.job.title,
      meta: [
        { hid: 'description', name: 'description', content: this.job.description }
      ]
    }
  },
  mounted () {
    console.log('mounted')
    const anime = this.$anime
    this.$nextTick(() => {
      anime({
        targets: '.title .mask',
        duration: 600,
        easing: 'easeInExpo',
        translateX: ['-100%', '0%'],
        complete () {
          anime.set('.title h1', {
            opacity: 1
          })
          anime({
            targets: '.title .mask',
            duration: 600,
            easing: 'easeOutExpo',
            translateX: ['0%', '100%']
          })
        }
      })
      anime({
        targets: '.subtitle .mask',
        duration: 600,
        easing: 'easeInExpo',
        delay: 200,
        translateX: ['-100%', '0%'],
        complete () {
          anime.set('.subtitle h4', {
            opacity: 1
          })
          anime({
            targets: '.subtitle .mask',
            duration: 600,
            easing: 'easeOutExpo',
            translateX: ['0%', '100%']
          })
        }
      })
    })
  }
}
</script>

<style lang="scss">
#work {
  z-index: 5;
  position: relative;
  padding: 160px 0 60px 0;
  .caption {
    position: relative;
    display: inline-block;
    overflow: hidden;
  }
  h1 {
    opacity: 0;
    margin: 0;
    font: 800 90px/90px 'Trump Gothic East', sans-serif;
    text-transform: uppercase;
    color: #eee;
  }
  h4 {
    opacity: 0;
    display: inline-block;
    position: relative;
    margin: 0;
    font: 800 22px/22px 'Trump Gothic East', sans-serif;
    color: #eee;
    letter-spacing: 1px;
    text-transform: uppercase;
  }
  .mask {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: #eee;
  }

  .gallery {
    margin-top: 3rem;
    .browser-mockup {
      margin-top: 2rem;
      position: relative;
      display: block;
      border: 2px solid rgba(255, 255, 255, 0.2);
      border-radius: 4px;
      overflow: hidden;
      img {
        width: 100%;
        border-top: 2px solid rgba(255, 255, 255, 0.2);
      }
      &:before {
        position: relative;
        content: ". . .";
        color: rgba(255, 255, 255, 0.2);
        font-size: 71px;
        line-height: 0;
        letter-spacing: -25px;
        left: 2px;
        top: -5px;
        width: 100%;
      }
    }
  }
}
</style>
