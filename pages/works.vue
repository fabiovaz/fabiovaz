<template>
  <div id="works">
    <div class="jobs">
      <NuxtLink v-for="job in jobs" :key="job.slug" :to="`/work/${job.slug}`" class="job">
        <img :src="job.cover.formats.small.url">
        <div class="caption">
          <h4>{{ job.title }}</h4>
        </div>
      </NuxtLink>
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
  }
}
</script>

<style lang="scss">
#works {
  z-index: 5;
  position: relative;
  margin: 0 60px;
  padding: 160px 0 60px 0;
  text-align: center;
  .jobs {
    display: grid;
    grid-template-columns: repeat(auto-fill,minmax(270px,1fr));
    grid-auto-rows: auto;
    grid-gap: 32px;
    .job {
      position: relative;
      background: #333;
      width: 100%;
      img {
        object-fit: cover;
        width: 100%;
        opacity: .5;
      }
      .caption {
        opacity: 0;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        text-align: center;
        color: #eee;
        text-transform: uppercase;
        transition: all .3s;
        h4 {
          font-family: 'Trump Gothic East', sans-serif;
        }
      }
      &:hover {
        .caption {
          opacity: .8;
          text-shadow: 0 0 10px #eee, 0 0 20px #eee, 0 0 30px #eee, 0 0 40px #eee, 0 0 70px #eee;
        }
      }
    }
  }
}
</style>
