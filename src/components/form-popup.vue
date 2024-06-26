<script setup>
import { ref, computed } from "vue";

const props = defineProps(["infosprop"]);

const infos = computed(() => {
  return props.infosprop;
});

const emit = defineEmits(["submit", "dimiss"]);
</script>
<template>
  <div
    class="bottom-0 left-0 right-0 top-0 backdrop-blur-sm bg-[rgba(0,_0,_0,_0.2)] absolute flex item-center justify-center"
  >
    <div class="w-1/2 h-[100vh] p-4 bg-[#1a2230] mt-20">
      <form
        v-on:submit.prevent="
          () => {
            $emit('submit', infos);
          }
        "
        class="w-full flex flex-col max-h-full h-full justify-between"
      >
        <label for="name" class="text-white mb-1 text-xs">Name</label>
        <input
          required
          name="name"
          v-model="infos.name"
          class="bg-[#0d141e] outline-none border-[#414446] border rounded-sm p-2 caret-white text-white text-sm mb-4 w-full"
        />
        <label for="description" class="text-white mb-1 text-xs">Description</label>
        <textarea
          name="description"
          v-model="infos.description"
          class="bg-[#0d141e] outline-none border-[#414446] border rounded-sm p-2 caret-white text-white text-sm mb-4 w-full min-h-[100px]"
        />
        <label for="image" class="text-white mb-1 text-xs">Image</label>
        <input
          name="image"
          v-model="infos.image"
          class="bg-[#0d141e] outline-none border-[#414446] border rounded-sm p-2 caret-white text-white text-sm mb-4 w-full"
        />
        <label for="genres" class="text-white mb-1 text-xs">Genres</label>
        <select
          required
          multiple
          name="genres"
          v-model="infos.genres"
          class="bg-[#0d141e] outline-none border-[#414446] border rounded-sm p-2 caret-white text-white text-sm mb-4 w-full min-h-[100px]"
        >
          <option value="Action">Action</option>
          <option value="Aventure">Aventure</option>
          <option value="Drama">Drama</option>
          <option value="Crime">Crime</option>
          <option value="Horror">Horror</option>
        </select>
        <div class="flex items-center">
          <input
            type="checkbox"
            name="checkbox"
            v-model="infos.inTeathers"
            class="bg-[#0d141e] outline-none border-[#414446] border rounded-sm caret-white text-white text-sm"
          />
          <label for="checkbox" class="text-white text-sm ml-2"
            >In theaters</label
          >
        </div>
        <div class="w-full flex justify-between mt-4">
          <button
            class="py-2 px-2 bg-[#5c6574] border-[#969696] border text-white rounded-sm"
            @click="$emit('dimiss')"
            type="button"
          >
            cancel
          </button>

          <button
            class="py-2 px-2 bg-[#00a9ce] border-[#969696] border text-white rounded-sm"
            type="submit"
          >
            Create
          </button>
        </div>
      </form>
    </div>
  </div>
</template>
