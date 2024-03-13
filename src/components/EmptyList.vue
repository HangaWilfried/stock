<template>
  <div>
    <span>{{ t(description) }}</span>
  </div>
</template>

<script setup lang="ts">
import { useI18n } from "vue-i18n";

withDefaults(
  defineProps<{
    description?: string;
  }>(),
  {
    description: "emptyList",
  },
);

const { t } = useI18n({
  messages: {
    en: {
      emptyList: "Empty list",
    },
    fr: {
      emptyList: "Liste vide",
    },
  },
});
</script>
