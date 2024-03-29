<script setup lang="ts">
import { useI18n } from "vue-i18n";
import { ref, type Ref } from "vue";

import { THEME } from "@/utils/enum";

import TwButton from "@/components/TwButton.vue";
import TextField from "@/components/TextField.vue";
import TextareaField from "@/components/TextareaField.vue";
import CloseButton from "@/components/CloseButton.vue";
import ModalWrapper from "@/components/ModalWrapper.vue";
import { useApiStore } from "@/stores/api";
import type { Brand, Builder } from "@/domain/brand";

const api = useApiStore();

const emit = defineEmits<{
  (e: "close"): void;
  (e: "refresh"): void;
}>();

const { t } = useI18n({
  messages: {
    en: {
      title: "Brand",
      description: "Description",
      edit: "Edit",
      cancel: "Cancel",
      formTitle: "Add brand form",
    },
    fr: {
      title: "Marque",
      description: "Description",
      edit: "Editer",
      cancel: "Annuler",
      formTitle: "Formulaire d'ajout de marque",
    },
  },
});

const props = defineProps<{
  brand: Brand;
}>();

const brand = ref<Builder>({
  name: props.brand.name,
  description: props.brand.description,
}) as Ref<Builder>;

const close = (): void => {
  emit("close");
};

const isLoading = ref<boolean>(false);

const editBrand = async (): Promise<void> => {
  isLoading.value = true;

  const payload = props.brand;
  payload.name = brand.value.name;
  payload.description = brand.value.description;

  await api.editBrand(payload);
  emit("refresh");
  isLoading.value = false;
};
</script>

<template>
  <ModalWrapper>
    <section class="bg-white rounded-xl flex flex-col w-[90%] max-w-[500px]">
      <section
        class="font-semibold flex flex-col p-4 border-b border-dashed border-gray-200"
      >
        <CloseButton class="self-end" @click="close" />
        <div class="text-lg">{{ t("formTitle") }}</div>
      </section>
      <section class="p-5 flex flex-col gap-4">
        <TextField v-model="brand.name" :label="t('title')" />
        <TextareaField v-model="brand.description" :label="t('description')" />
        <div class="flex gap-2 justify-end items-center">
          <TwButton
            :cta="t('cancel')"
            :theme="THEME.BORDER_BLUE"
            @click="close"
          />
          <TwButton :cta="t('edit')" :theme="THEME.BLUE" @click="editBrand" />
        </div>
      </section>
    </section>
  </ModalWrapper>
</template>
