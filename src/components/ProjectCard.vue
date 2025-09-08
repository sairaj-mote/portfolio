<script setup>
import { useIntersectionObserver } from '@vueuse/core'
import { ExternalLink, ChevronRight } from 'lucide-vue-next'
import { ref } from 'vue'
import { RouterLink } from 'vue-router'

const { title, tags, link, subtitle, id, platform } = defineProps({
  title: String,
  thumbnail: String,
  tags: Array,
  link: String,
  subtitle: String,
  id: String,
  platform: String,
  heroImage: String,
})

const target = ref(null)
const targetIsVisible = ref(false)

const { stop } = useIntersectionObserver(
  target,
  ([entry]) => {
    targetIsVisible.value = entry?.isIntersecting || false
    if (entry.isIntersecting) {
      stop()
    }
  },
  {
    threshold: 0.2,
  },
)
</script>
<template>
  <li
    ref="target"
    :key="title"
    class="relative flex flex-col md:flex-row-reverse gap-4 rounded-xl transition duration-500 ease-in-out md:bg-gray-50 md:p-6 md:border-1 border-gray-200"
    :class="`${targetIsVisible ? 'opacity-100 translate-y-0 scale-100' : 'opacity-0 translate-y-8 scale-95'}`"
  >
    <div class="flex relative flex-1">
      <img class="w-full object-contain rounded-xl" :src="heroImage" :alt="title" loading="lazy" />
    </div>
    <div class="flex flex-col gap-4 flex-1">
      <RouterLink :to="`/projects/${id}`" class="hover:underline">
        <h2 class="text-2xl font-bold">
          {{ title }}
        </h2>
      </RouterLink>
      <div class="flex gap-1 flex-wrap">
        <p
          class="text-sm bg-gray-200 py-1 px-2 rounded text-gray-700"
          v-for="tag in tags"
          :key="tag"
        >
          {{ tag }}
        </p>
      </div>
      <p class="text-gray-700 text-lg whitespace-pre-line">{{ subtitle }}</p>
      <div class="flex gap-2">
        <a
          v-if="link"
          :href="link"
          target="_blank"
          class="flex items-center gap-2 button self-start"
          >Live
          <ExternalLink class="inline-block" size="20" />
        </a>
        <RouterLink :to="`/projects/${id}`" class="flex items-center gap-2 button self-start">
          Case Study
          <ChevronRight class="-mr-2" />
        </RouterLink>
      </div>
    </div>
  </li>
</template>
