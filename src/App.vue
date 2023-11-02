<template>
  <main class="max-w-2xl mx-auto">
    <BeerList />
  </main>
</template>

<script setup lang="ts">
import BeerList from "@/components/BeerList.vue";

/**
 * Your task is to implement a virtualized list of beers.
 *
 * 1. Use the `BeerListItem` component to render each beer. Start with the sample data in `./data/beers.ts`.
 * 2. Fetch beers from the BeersAPI (https://api.punkapi.com/v2/beers). You can use the `fetch` API or any other library.
 * 3. Use the `useVirtualizer` composable to virtualize the list. See the example in `./examples/RowVirtualizerExample.vue`.
 * 4. Load more beers, when the user scrolls to the bottom of the list.
 */
</script>
