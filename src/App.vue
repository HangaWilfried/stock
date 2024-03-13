<script setup lang="ts">
import { RouterView } from "vue-router";
import { ref, onMounted } from "vue";

const navBar = ref<HTMLElement | undefined>(undefined);
const height = ref<string | undefined>(undefined);

onMounted(() => {
  height.value = navBar.value?.getBoundingClientRect().height + "px";
});
</script>

<template>
  <section>
    <header ref="navBar"></header>
    <main class="--base-height">
      <RouterView></RouterView>
    </main>
  </section>
</template>

<style scoped>
.--base-height {
  height: calc(100vh - v-bind(height));
}
</style>
