<script setup>
import { StarIcon, TrashIcon, PencilIcon } from "@heroicons/vue/24/solid";
import StarButton from "./star-button.vue";

defineProps(["imgSrc", "title", "desc", "rating", "tags", "id"]);

const emit = defineEmits(["exclude", "edit", "changeRating"]);



</script>

<template>
  <div class="w-full rounded-md overflow-hidden relative">
    <div class="absolute right-0">
      <div class="relative">
        <StarIcon
          class="w-16 text-yellow-500"
          :class="{
            'text-yellow-500': rating != 0,
            'text-gray-500': rating == 0,
          }"
        ></StarIcon>
        <h1
          class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-fit font-bold"
          :class="{
            'text-yellow-800': rating != 0,
            'text-gray-300': rating == 0,
          }"
        >
          {{ rating != 0 ? rating : "-" }}
        </h1>
      </div>
    </div>
    <img :src="imgSrc" class="h-[470px] object-cover w-full object-top" />
    <div class="bg-white p-4 flex justify-between flex-col h-[210px]">
      <div>
        <h1 class="text-xl font-semibold">{{ title }}</h1>
        <div class="flex space-x-1">
          <h2
            v-for="tag in tags"
            class="bg-[#6366f1] text-white w-fit px-2 rounded-full text-xs"
            :key="tag"
          >
            {{ tag }}
          </h2>
        </div>

        <p class="text-sm mt-2 line-clamp-3">{{ desc }}</p>
      </div>
      <div class="flex w-full justify-between items-center">
        <span class="flex text-sm"
          >Rating: ({{ rating }}/5)
          <StarButton
            class="text-[#ebb30f] w-5"
            v-for="index in 5"
            @starClicked="$emit('changeRating', {id, index})"
            :index="index"
            :currentRating="rating"
        /></span>
        <div class="flex space-x-2">
          <button
            class="bg-gray-200 p-2 rounded-full"
            @click="$emit('exclude', id)"
          >
            <TrashIcon class="w-4 text-gray-600" />
          </button>
          <button
            class="bg-[#6366f1] p-2 rounded-full"
            @click="$emit('edit', id)"
          >
            <PencilIcon class="w-4 text-gray-50" />
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.line-clamp-3 {
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 3;
  overflow: hidden;
}
</style>
