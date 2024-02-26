<script setup lang="ts">
const props = defineProps({
  avatarImage: { type: String },
  name: { type: String },
  message: { type: String },
  type: { type: String },
});

</script>

<template>
  <div
    class="rounded-lg"
    :class="props.type === 'right' ? 'bg-slate-400' : 'bg-slate-600'"
  >
    <div class="flex align-items-center pl-2 w-full h-24">
      <img :src="props.avatarImage" class="w-20" />
      <div class="flex flex-col">
        <span class="font-bold ml-2 "> {{ props.name }}</span>
        <p class="ml-5">
          {{ props.message }}
        </p>
      </div>
    </div>
  </div>
</template>

<style>
/* modo de comunicar props e css mais indicado quando não há tantas variações
    .p-fieldset{
        background-color: v-bind(cardStyle.backColor);
        font-size: v-bind(cardStyle.fontSize);
        color: v-bind(cardStyle.textColor);
    }*/
</style>
