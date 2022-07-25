<template>
  <component
    :is="el"
    :href="href"
    :target="href ? '_blank' : null"
    class="anchor"
    @click="handler"
  >
    <slot />
    <span class="button button__top">
      <span>
        <slot />
      </span>
    </span>
    <span class="button button__bottom">
      <span>
        <slot />
      </span>
    </span>
  </component>
</template>

<script>
export default {
  name: 'LinkComponent',
  props: {
    href: {
      type: String,
      default: null
    }
  },
  computed: {
    el () {
      if (this.href) {
        return 'a'
      } else {
        return 'button'
      }
    }
  },
  methods: {
    handler (e) {
      this.$emit(e.type, e)
    }
  }
}
</script>

<style scoped lang="scss">
.anchor {
  display: inline-block;
  position: relative;
  color: transparent;
  cursor: pointer;
  font-family: 'Trump Gothic East', sans-serif;
  letter-spacing: 6px;
  text-transform: uppercase;
  &::before {
    z-index: 1;
    content: '';
    display: block;
    position: absolute;
    top: 50%;
    left: -10px;
    right: -8px;
    height: 1px;
    border-radius: 0px;
    background: #eee;
    transform: scale(0);
    transition: transform .8s cubic-bezier(.16,1.08,.38,.98);
  }
  .button {
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
    .button {
      color: #FFF;
      &__top {
        transform: skewX(12deg) translateX(1px);
      }
      &__bottom {
        transform: skewX(12deg) translateX(-1px);
      }
    }
    &::before { transform: scale(1); }
  }
}
</style>
