<template>
  <div>
    <div id="error-effect" />
    <div id="error-glitch" />
    <Header />
    <Nuxt />
  </div>
</template>

<script>
export default {}
</script>

<style lang="scss">
body {
  background: #222;
  color: #eee;
  font-family: 'Inconsolata';
}
#error-effect {
  z-index: 4;
  position: absolute;
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
    position: absolute;
    width: 100%;
    height: 100%;
    background: radial-gradient(rgba(0, 0, 0, 0) 50%, rgba(0, 0, 0, 0.8)), linear-gradient(rgba(255, 255, 255, 0.4) 10%, transparent);
  }
}
#error-glitch {
  z-index: 5;
  position: absolute;
  width: 100%;
  height: 100%;
  &:after {
    content: " ";
    position: absolute;
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
</style>
