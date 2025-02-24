﻿<template>
  <div ref="heroBackground" :style="backgroundStyle" class="hero-background">
    <slot />
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, computed } from "vue";

import gamePreview01 from "@/public/img/game_preview/game_preview_01.avif";
import gamePreview02 from "@/public/img/game_preview/game_preview_02.avif";
import gamePreview03 from "@/public/img/game_preview/game_preview_03.avif";
import gamePreview05 from "@/public/img/game_preview/game_preview_05.avif";
import gamePreview06 from "@/public/img/game_preview/game_preview_06.avif";
import gamePreview07 from "@/public/img/game_preview/game_preview_07.avif";

const images = [
  gamePreview01,
  gamePreview02,
  gamePreview03,
  gamePreview05,
  gamePreview06,
  gamePreview07,
];

const heroBackground = ref(null);
const isImageLoaded = ref(false);
const currentImage = ref(images[0]);
let currentIndex = 0;

const backgroundStyle = computed(() => ({
  backgroundImage: isImageLoaded.value ? `url(${currentImage.value})` : "none",
  backgroundColor: "#000", // Fallback color while image is loading
  height: "100vh", // Ensure the background takes up the full viewport height
}));

const preloadImages = () => {
  images.forEach((src) => {
    const img = new Image();
    img.onload = () => {
      isImageLoaded.value = true;
    };
    img.src = src;
  });
};

const changeBackground = () => {
  currentIndex = (currentIndex + 1) % images.length;
  currentImage.value = images[currentIndex];
};

const updateHeight = () => {
  if (heroBackground.value) {
    heroBackground.value.style.height = `${window.innerHeight}px`;
  }
};

onMounted(() => {
  preloadImages();
  setInterval(changeBackground, 5000);
  updateHeight();
  window.addEventListener("resize", updateHeight);
});

onUnmounted(() => {
  window.removeEventListener("resize", updateHeight);
});
</script>

<style scoped lang="scss">
.hero-background {
  position: relative;
  width: 100%;
  background-size: cover;
  background-position: center;
  transition: background-image 1s ease-in-out;

  @media only screen and (max-width: 600px) {
    &::before {
      backdrop-filter: blur(10px);
    }
  }

  &::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: linear-gradient(
      to bottom,
      #000f 0%,
      #0000 100px,
      #0000 calc(100% - 100px),
      #000f 100%
    );
    pointer-events: none;
  }
}
</style>
