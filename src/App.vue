<template>
  <div class="flex w-full px-20 justify-between mt-10 items-center">
    <h1 class="text-white">
      Total Movies: {{ movies.length }} / Average Rating:
      {{
        (
          movies.map((c) => c.rating).reduce((a, b) => a + b) / movies.length
        ).toFixed(1)
      }}
    </h1>
    <div class="flex space-x-3">
      <button
        @click="resetRatings"
        class="py-2 px-2 bg-[#00a9ce] border-[#969696] border rounded-sm text-white"
      >
        Remove Ratings
      </button>
      <button
        @click="show = true"
        class="py-2 px-2 bg-[#00a9ce] border-[#969696] border rounded-sm text-white"
      >
        Add movie
      </button>
    </div>
  </div>

  <div class="grid grid-cols-3 grid-rows-1 gap-4 p-20 py-10">
    <movie
      v-for="mov in compmovies"
      :key="mov.id"
      :img-src="mov.image"
      :title="mov.name"
      :desc="mov.description"
      :rating="mov.rating"
      :tags="mov.genres"
      :id="mov.id"
      @exclude="excludeMovie"
      @edit="editMovie"
      @changeRating="changeRating"
    />
  </div>

  <FormPopup
    v-show="show"
    @submit="saveMovie"
    @dimiss="dismissForm"
    :infosprop="infos"
  ></FormPopup>
</template>

<script setup>
import { reactive, ref, computed } from "vue";
import { items } from "./movies.json";
import movie from "./components/movie.vue";
import FormPopup from "./components/form-popup.vue";

const movies = reactive([...items]);
const show = ref(false);

const compmovies = computed(() => {
  return movies;
});

const infos = ref({
  id: undefined,
  name: "",
  description: "",
  image: "",
  inTheaters: [],
  genres: [],
});

const resetRatings = () => {
  movies.forEach((c, i) => {
    movies[i] = {
      ...movies[i],
      rating: 0,
    };
  });
};

const changeRating = ({ id, index }) => {
  const movieIndex = movies.findIndex((c) => c.id === id);

  if (movieIndex !== -1) {
    // Update existing movie
    movies[movieIndex] = {
      ...movies[movieIndex],
      rating: index,
    };
  }
};

const excludeMovie = (id) => {
  const index = movies.findIndex((c) => c.id === id);
  if (index !== -1) {
    movies.splice(index, 1);
  }
};

const saveMovie = (infosparam) => {
  const movieIndex = movies.findIndex((c) => c.id === infosparam.id);

  if (movieIndex !== -1) {
    // Update existing movie
    movies[movieIndex] = {
      ...movies[movieIndex],
      name: infosparam.name,
      description: infosparam.description,
      image: infosparam.image,
      inTheaters: !!infosparam.inTheaters,
      genres: infosparam.genres,
    };
  } else {
    // Add new movie
    const newId = Math.max(...movies.map((a) => a.id)) + 1;
    movies.push({
      id: newId,
      name: infosparam.name,
      description: infosparam.description,
      image: infosparam.image,
      inTheaters: !!infosparam.inTheaters,
      genres: infosparam.genres,
      rating: 0,
    });
  }

  dismissForm();
};

const editMovie = (id) => {
  const movieToEdit = movies.find((c) => c.id == id);
  if (movieToEdit) {
    infos.value = { ...movieToEdit };
    show.value = true;
  }
};

const dismissForm = () => {
  show.value = false;
  infos.value = {
    id: undefined,
    name: "",
    description: "",
    image: "",
    inTheaters: [],
    genres: [],
  };
};
</script>
