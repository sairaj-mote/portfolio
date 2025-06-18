<script setup>
import { useIntersectionObserver } from '@vueuse/core'
import { ExternalLink, ChevronRight } from 'lucide-vue-next'
import { ref } from 'vue'
import { RouterLink } from 'vue-router'

const { name, thumbnail, tags, link, description, id, platform } = defineProps({
  name: String,
  thumbnail: String,
  tags: Array,
  link: String,
  description: String,
  id: String,
  platform: String,
})

const target = ref(null)
const targetIsVisible = ref(false)

const { stop } = useIntersectionObserver(
  target,
  ([entry], observerElement) => {
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
    :key="name"
    class="flex flex-col gap-4 rounded-xl border p-4 md:p-6 break-inside-avoid mb-4 transition duration-500 ease-in-out"
    :class="`${targetIsVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-8'}`"
  >
    <div class="flex relative">
      <img
        class="max-w-[12rem] w-full aspect-square bg-gray-100 object-contain p-8 rounded-xl"
        :src="thumbnail"
        :alt="name"
      />
      <img
        class="absolute top-0 right-0 w-16 h-16 bg-gray-100 object-contain p-4 rounded-xl"
        :src="`/icons/platforms/${platform}.svg`"
        :alt="`${platform} icon`"
      />
    </div>
    <h2 class="text-2xl font-bold">
      {{ name }}
    </h2>
    <div class="flex gap-1 flex-wrap">
      <p class="text-sm bg-gray-200 py-1 px-2 rounded text-gray-700" v-for="tag in tags" :key="tag">
        {{ tag }}
      </p>
    </div>
    <p class="text-gray-700 text-lg whitespace-pre-line">{{ description }}</p>
    <div class="flex gap-2">
      <a v-if="link" :href="link" target="_blank" class="flex items-center gap-2 button self-start"
        >Live
        <ExternalLink class="inline-block" size="20" />
      </a>
      <RouterLink :to="`/projects/${id}`" class="flex items-center gap-2 button self-start">
        Read more
        <ChevronRight class="-mr-2" />
      </RouterLink>
    </div>
  </li>
</template>
