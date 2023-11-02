<template>
  <div class="flex justify-between gap-x-6 py-5">
    <div class="flex min-w-0 gap-x-4 items-center">
      <img
        class="h-36 w-36 flex-none rounded bg-gray-50 object-contain border"
        :src="beer.image_url"
        :alt="beer.name"
      />
      <div class="min-w-0 flex-auto">
        <p class="text-xl font-semibold leading-6 text-gray-900">
          {{ beer.name }}
        </p>
        <p class="mt-1 text-gray-500 line-clamp-3">{{ beer.description }}</p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import beers from "../data/beers";

defineProps<{
  beer: (typeof beers)[number];
}>();
</script>
