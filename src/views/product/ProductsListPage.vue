<template>
  <div>
    <InLoading v-if="isLoading" />
    <div
      v-else-if="products.length > 0"
      class="max-w-[1920px] m-auto grid grid-cols-2 md:grid-cols-3 lg:grid-col-4 xl:grid-col-5 gap-2"
    >
      <ProductCard
        v-for="product in products"
        :key="product.id"
        :product="product"
        :callback="goToProductDetails"
      />
    </div>
    <EmptyList v-else />
  </div>
</template>

<script setup lang="ts">
import InLoading from "@/components/InLoading.vue";
import type { Product } from "@/domain/product";
import { useApiStore } from "@/stores/api";
import { defineAsyncComponent, onBeforeMount, ref, type Ref } from "vue";
import { useRouter } from "vue-router";

const EmptyList = defineAsyncComponent(
  () => import("@/components/EmptyList.vue"),
);
const ProductCard = defineAsyncComponent(
  () => import("@/components/ProductCard.vue"),
);

const api = useApiStore();

const isLoading = ref<boolean>(false);
const products = ref([]) as Ref<Product[]>;

const router = useRouter();
const goToProductDetails = async (productId: string) => {
  await router.push(`products/${productId}`);
};

onBeforeMount(async () => {
  isLoading.value = true;
  products.value = await api.getAllProducts();
  isLoading.value = false;
});
</script>
