<script setup lang="ts">
import type { MarkdownRoot } from "@nuxt/content";

const { body } = defineProps<{
  body: MarkdownRoot;
}>();
</script>

<template>
  <ContentRendererMarkdown
    :value="body"
    class="content-renderer"
  />
</template>

<style scoped lang="scss">
.content-renderer {
  color: #fff;
  font-size: 16px;
  width: 100%;
  margin: 40px auto;

  :deep(h1) {
    font-size: 32px;
    font-weight: 800;
    margin: 20px 0;
  }

  :deep(h2) {
    font-size: 25px;
    font-weight: 800;
    margin: 20px 0;
  }

  :deep(h3) {
    font-size: 20px;
    font-weight: 800;
    margin: 20px 0;
  }

  :deep(h4) {
    font-size: 17px;
    font-weight: 800;
    margin: 20px 0;
  }

  :deep(h5) {
    font-size: 15px;
    font-weight: 800;
    margin: 20px 0;
  }

  :deep(p) {
    margin-bottom: 20px;
  }

  :deep(a) {
    color: #3388ff;
    font-weight: bold;
  }

  :deep(h2 a) {
    color: white;
    font-weight: bold;
  }

  :deep(h3 a) {
    color: white;
    font-weight: bold;
  }

  :deep(ul) {
    margin-left: 1.5rem;
    list-style-type: circle;
  }

  :deep(li) {
    margin-bottom: 1rem;
  }
}
</style>