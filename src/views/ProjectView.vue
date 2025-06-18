<script setup>
import { ref } from 'vue'
import projects from '../data/projects.json'
import { useRoute } from 'vue-router'
import { ExternalLink, ArrowLeft } from 'lucide-vue-next'

const route = useRoute()
const project = projects.find((p) => p.id == route.params.id)
const {
  id,
  name,
  image,
  tags,
  description,
  link,
  heroImage,
  role,
  overview,
  platform,
  url,
  highlights,
  takeaways,
  contributions,
} = project
const isMobileView = ref(false)
const mobileViewMediaQuery = window.matchMedia('(max-width: 768px)')
isMobileView.value = mobileViewMediaQuery.matches
mobileViewMediaQuery.addEventListener('change', (e) => {
  isMobileView.value = e.matches
})
</script>
<template>
  <div class="flex flex-col bg-white items-center">
    <!-- Title & Link -->
    <div
      class="flex flex-col gap-4 p-6 md:px-16 md:flex-row justify-between items-start md:items-center w-full sticky top-0 bg-white/90 backdrop-blur-lg z-10"
    >
      <RouterLink to="/" class="button flex gap-2 items-center">
        <ArrowLeft />
      </RouterLink>

      <h2 class="w-full text-2xl font-bold text-gray-800">{{ name }}</h2>
      <a v-if="!isMobileView" :href="link" target="_blank" class="flex-shrink-0 button">
        View Project <ExternalLink class="inline-block" size="20" />
      </a>
    </div>
    <div
      class="flex md:grid md:grid-cols-2 flex-col gap-8 flex-1 md:px-6 items-center md:items-start"
    >
      <img
        :src="heroImage"
        :alt="name"
        class="w-full mb-8 md:rounded-2xl max-w-2xl md:shadow-xl flex-1 flex-shrink object-cover md:sticky md:top-[8rem]"
      />
      <div class="flex flex-col gap-8 max-w-2xl pb-24 px-6 flex-1">
        <!-- Roles -->
        <div class="flex flex-col gap-2 text-lg">
          <h3 class="font-semibold text-gray-700">Role</h3>
          <ul class="flex flex-wrap gap-2">
            <li class="bg-gray-100 text-gray-700 font-medium px-2 py-1 rounded-md">
              {{ role }}
            </li>
          </ul>
        </div>

        <div class="text-lg flex flex-col gap-2">
          <h3 class="font-semibold text-gray-700">Platform</h3>
          <div class="flex">
            <img class="w-16" :src="`/icons/platforms/${platform}.svg`" :alt="`${platform} icon`" />
          </div>
        </div>

        <!-- Overview -->
        <div class="text-lg flex flex-col gap-2">
          <h3 class="font-semibold text-gray-700">Project Overview</h3>
          <p class="text-gray-700 leading-relaxed text-lg">{{ overview }}</p>
        </div>

        <!-- Contributions -->
        <div class="text-lg flex flex-col gap-2">
          <h3 class="font-semibold text-gray-700">My Contributions</h3>
          <ul class="flex flex-col gap-2 list-disc ml-6 text-gray-700 space-y-1">
            <li v-for="(desc, title) in contributions" :key="title">
              <strong>{{ title }}:</strong> {{ desc }}
            </li>
          </ul>
        </div>

        <!-- Highlights -->
        <div class="text-lg flex flex-col gap-2">
          <h3 class="font-semibold text-gray-700">Highlights</h3>
          <ul class="flex flex-col gap-2 list-disc ml-6 text-gray-700 space-y-1">
            <li v-for="(highlight, index) in highlights" :key="index">{{ highlight }}</li>
          </ul>
        </div>

        <!-- Takeaways -->
        <div class="text-lg flex flex-col gap-2">
          <h3 class="font-semibold text-gray-700">Key Takeaways</h3>
          <p class="text-gray-700 leading-relaxed">{{ takeaways }}</p>
        </div>
      </div>
      <a v-if="isMobileView" :href="link" target="_blank" class="fixed bottom-0 m-6 button">
        View Project <ExternalLink class="inline-block" size="20" />
      </a>
    </div>
  </div>
</template>
