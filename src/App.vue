<template>
  <div ref="parentRef" class="h-screen overflow-auto">
    <ul
      class="relative divide-y max-w-2xl mx-auto"
      :style="{ height: `${totalSize}px` }"
    >
      <li
        v-for="virtualRow in virtualRows"
        :key="virtualRow.index"
        :style="{
          position: 'absolute',
          top: 0,
          left: 0,
          width: '100%',
          height: `${virtualRow.size}px`,
          transform: `translateY(${virtualRow.start}px)`,
        }"
      >
        <!--
          Using IntersectionOberserver to track, if one of the last
          current beers is in the viewport. If yes, then we can fetch the
          next beers. -->
        <template v-if="virtualRow.index > beers.length - 3">
          <BeerCard
            v-intersection-observer="onIntersection"
            :beer="beers[virtualRow.index]"
          />
        </template>
        <template v-else>
          <BeerCard :beer="beers[virtualRow.index]" />
        </template>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import BeerCard from "@/components/BeerCard.vue";
import beerData from "@/data/beers";
import { computed, ref } from "vue";
import { useVirtualizer } from "@tanstack/vue-virtual";
import { vIntersectionObserver } from "@vueuse/components";

// beers that are currently loaded
const beers = ref<(typeof beerData)[number][]>([]);

const isLoading = ref(false);
const page = ref(1);

// fetches next 10 beers from API and adds them to beers ref
const baseURI = "https://api.punkapi.com/v2/beers";
function getNextBeers() {
  isLoading.value = true;
  fetch(`${baseURI}?page=${page.value}&per_page=${10}`)
    .then((res) => res.json())
    .then((nextBeers) => {
      ++page.value;
      beers.value.push(...nextBeers);
    })
    .finally(() => (isLoading.value = false));
}

// triggered when the HTML element is intersecting with the viewport
function onIntersection([{ isIntersecting }]: IntersectionObserverEntry[]) {
  // only fetch next beers if the element is intersecting and is not loading
  if (isIntersecting === true && isLoading.value === false) getNextBeers();
}

const parentRef = ref<Element | null>(null);
// IMPORTANT: the virtualizerOptions must be reactive, so that count can be tracked
const virtualizerOptions = computed(() => {
  return {
    count: beers.value.length,
    getScrollElement: () => parentRef.value,
    estimateSize: () => 184,
    overscan: 5,
  };
});

// Pass virtualizerOptions to virtualizer. Don't use `.value` here.
// Otherwise the virtualizer won't update when count changes.
const rowVirtualizer = useVirtualizer(virtualizerOptions);
const virtualRows = computed(() => rowVirtualizer.value.getVirtualItems());
const totalSize = computed(() => rowVirtualizer.value.getTotalSize());

// fetch first 10 beers
getNextBeers();

/**
 * Your task is to implement a virtualized list of beers.
 *
 * 1. Use the `BeerListItem` component to render each beer. Start with the sample data in `./data/beers.ts`.
 * 2. Fetch beers from the BeersAPI (https://api.punkapi.com/v2/beers). You can use the `fetch` API or any other library.
 * 3. Use the `useVirtualizer` composable to virtualize the list. See the example in `./examples/RowVirtualizerExample.vue`.
 * 4. Load more beers, when the user scrolls to the bottom of the list.
 */
</script>
