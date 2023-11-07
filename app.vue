<template>
  <div class="sg-background" @mousemove="onMouseMove" :style="{
    filter,
    backgroundPosition: (mouseX * -100) + 'px ' + (mouseY * -100) + 'px'
  }">
  </div>
  <img src="/img/logo.svg" :alt="title" class="sg-logo" :style="{ transform }">
</template>

<script>
export default {
  data() {
    return {
      title: 'Savage Grace Organics',
      mouseX: 0,
      mouseY: 0
    };
  },
  methods: {
    onMouseMove(e) {
      this.mouseX = (e.clientX / window.innerWidth) - 0.5;
      this.mouseY = (e.clientY / window.innerHeight) - 0.5;

      console.log(this.mouseX, this.mouseY);
    }
  },
  computed: {
    filter() {
      const grayscale = Math.abs(this.mouseX * 100);
      return `grayscale(${grayscale}%)`;
    },
    transform() {
      const rotateY = this.mouseX * 30;
      const rotateX = this.mouseY * -30;
      return `rotateY(${rotateY}deg) rotateX(${rotateX}deg) translateZ(100px) translate(-50%, -50%)`;
    }
  },
  mounted() {}
};
</script>

<style lang="scss" scoped>

  .sg-background {
    background-image: url("/img/slide-1.jpg");
    background-size: cover;
    height: 120vh;
    width: 120vw;
    top: -10vh;
    left: -10vw;
    position: absolute;
    overflow: hidden;
    display: flex;
    align-items: center;
    justify-content: center;

    &::after {
      content: "";
      background-color: #b14499;
      mix-blend-mode: hard-light;
      position: absolute;
      top: 0;
      left: 0;
      height: 100%;
      width: 100%;
    }
  }

  

  .sg-logo {
    opacity: 0.8;
    pointer-events: none;
    position: absolute;
    top: 50%;
    left: 50%;
    width: 50%;
    height: auto;
    max-width: 500px;
    perspective: 2000px;
    perspective-origin: 50% 50%;
  }
</style>
