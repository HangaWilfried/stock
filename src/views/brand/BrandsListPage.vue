<template>
  <div>
    <InLoading v-if="isLoading" />
    <div
      v-else-if="brands.length > 0"
      class="max-w-[1920px] m-auto grid grid-cols-2 md:grid-cols-3 lg:grid-col-4 xl:grid-col-5 gap-2"
    >
      <BrandCard
        v-for="brand in brands"
        :key="brand.id"
        :brand="brand"
        :callback="goToBrandDetails"
      />
    </div>
    <EmptyList v-else />
  </div>
</template>

<script setup lang="ts">
import type { Brand } from "@/domain/brand";
import { useApiStore } from "@/stores/api";
import { defineAsyncComponent, onBeforeMount, ref, type Ref } from "vue";
import { useRouter } from "vue-router";

const EmptyList = defineAsyncComponent(
  () => import("@/components/EmptyList.vue"),
);
const BrandCard = defineAsyncComponent(
  () => import("@/components/BrandCard.vue"),
);

const api = useApiStore();

const isLoading = ref<boolean>(false);
const brands = ref([]) as Ref<Brand[]>;

const router = useRouter();
const goToBrandDetails = async (brandId: string) => {
  await router.push(`brands/${brandId}`);
};

onBeforeMount(async () => {
  isLoading.value = true;
  brands.value = await api.getAllBrands();
  isLoading.value = false;
});
</script>
