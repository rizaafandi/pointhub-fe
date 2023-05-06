<script setup lang="ts">
import { RouterView } from "vue-router";
import { Icon } from "@iconify/vue";
</script>

<template>
  <div class="text-gray-600 flex flex-col">
    <header
      class="flex flex-row justify-between items-center border border-gray-200"
    >
      <div class="flex flex-row items-center">
        <div class="bg-gray-600 text-white p-3">
          <Icon icon="mdi:menu" class="h-9 w-9" />
        </div>
        <div class="px-2">
          <Icon icon="mdi:image-filter-hdr" class="h-9 w-9" />
        </div>
      </div>
      <div class="flex flex-row gap-2 items-center px-2">
        <div class="flex flex-col justify-start">
          <h1 class="text-2xl uppercase">renee mckelvey</h1>
          <span class="text-xl capitalize">account settings</span>
        </div>
        <Icon icon="mdi:account" class="h-9 w-9" />
      </div>
    </header>
    <div class="flex flex-row">
      <div class="flex flex-col bg-gray-200 gap-5 h-screen py-5 px-3">
        <Icon icon="mdi:search" class="h-9 w-9" />
        <Icon icon="mdi:home" class="h-9 w-9" />
        <Icon icon="mdi:file-chart-outline" class="h-9 w-9" />
        <Icon icon="mdi:paper" class="h-9 w-9" />
        <Icon icon="mdi:account-supervisor" class="h-9 w-9" />
        <Icon icon="mdi:cog" class="h-9 w-9" />
      </div>
      <div class="w-full flex">
        <div class="w-2/4 mx-auto">
          <RouterView />
        </div>
      </div>
    </div>
  </div>
</template>
