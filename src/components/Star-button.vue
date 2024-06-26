<script setup>
import { StarIcon } from "@heroicons/vue/24/solid";
import { computed, ref, toRefs } from "vue";

const props = defineProps(["index", "currentRating"]);

const color = computed(() => {
  return props.index <= props.currentRating;
});
</script>

<template>
  <StarIcon
    class="w-5"
    @click="
      props.index != props.currentRating ? $emit('starClicked', index) : null
    "
    :class="{ 'text-yellow-500': color, 'text-gray-500': !color }"
  />
</template>
