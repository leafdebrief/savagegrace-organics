<template>
  <div 
    class="sg-background" 
    @mousemove="onMouseMove" 
    :style="{
      filter,
      backgroundPosition: (mouseX * -100) + 'px ' + (mouseY * -100) + 'px'
    }"
  ></div>
  <div 
    :style="transform"
  >
    <img
      src="/img/logo-background.svg" 
      alt="" 
      class="sg-logo sg-logo--background" 
    >
    <img
      :alt="title" 
      src="/img/logo-foreground.svg" 
      class="sg-logo sg-logo--foreground" 
    >
  </div>
  
</template>

<script setup lang="ts">
import { computed, onMounted, ref } from 'vue';

const title = ref('Savage Grace Organics');
const mouseX = ref(0);
const mouseY = ref(0);

useHead({
  title: title.value,
  meta: [
    { name: 'description', content: 'My amazing site.' }
  ]
});

const filter = computed(() => {
  const grayscale = Math.abs(mouseX.value * 100);
  return `grayscale(${grayscale}%)`;
});

const transform = computed(() => {
  const rotateY = Math.round(mouseX.value * 30);
  const rotateX = Math.round(mouseY.value * -30);
  return {
    '--rotate-y': `${rotateY}deg`,
    '--rotate-x': `${rotateX}deg`
  };
});

onMounted(() => {
  window.addEventListener('deviceorientation', onMouseMove.bind(this), true);
})

function onMouseMove(e) {
  if (e.type === 'deviceorientation') {
    mouseX.value = (e.gamma / 90) - 0.5;
    mouseY.value = (e.beta / 90) - 0.5;
    return;
  }

  mouseX.value = (e.clientX / window.innerWidth) - 0.5;
  mouseY.value = (e.clientY / window.innerHeight) - 0.5;

  console.log(mouseX.value, mouseY.value);
}
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
    pointer-events: none;
    position: absolute;
    top: 50%;
    left: 50%;
    width: 50%;
    height: auto;
    max-width: 500px;
    perspective: 2000px;
    perspective-origin: 50% 50%;
    transform: rotateY(var(--rotate-y, 0deg)) rotateX(var(--rotate-x, 0deg)) translateZ(var(--translate-z, 100px)) translate(-50%, -50%);
    transition: 0.1s;

    &--background {
      opacity: 0.8;
    }

    &--foreground {
      --translate-z: 150px;
    }
  }
</style>
