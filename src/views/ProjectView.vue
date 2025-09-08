<script setup>
import { ref, computed, onMounted, onUnmounted, useTemplateRef } from 'vue'
import { useScrollLock } from '@vueuse/core'
import { useRoute } from 'vue-router'
import { ExternalLink, ArrowLeft, X } from 'lucide-vue-next'
import projects from '../data/case-studies.json'

const route = useRoute()

const projectData = computed(() => {
  return projects.find((p) => p.id === route.params.id)
})

// --- Lightbox Logic ---
const lightboxImage = ref(null)
const isLocked = useScrollLock(document.body)

const openLightbox = (imageSrc) => {
  isLocked.value = true // lock
  lightboxImage.value = imageSrc
}

const closeLightbox = () => {
  isLocked.value = false // unlock
  lightboxImage.value = null
}

const handleKeydown = (e) => {
  if (e.key === 'Escape') {
    closeLightbox()
  }
}

// --- Responsive Logic ---
const isMobileView = ref(false)
let mobileViewMediaQuery = null
const mediaQueryListener = (e) => {
  isMobileView.value = e.matches
}

onMounted(() => {
  // Add keyboard listener for lightbox
  window.addEventListener('keydown', handleKeydown)

  // Setup media query listener
  mobileViewMediaQuery = window.matchMedia('(max-width: 768px)')
  isMobileView.value = mobileViewMediaQuery.matches
  mobileViewMediaQuery.addEventListener('change', mediaQueryListener)
})

onUnmounted(() => {
  // Clean up listeners to prevent memory leaks
  window.removeEventListener('keydown', handleKeydown)
  if (mobileViewMediaQuery) {
    mobileViewMediaQuery.removeEventListener('change', mediaQueryListener)
  }
})
</script>

<template>
  <div v-if="projectData" class="flex flex-col items-center">
    <header
      class="flex flex-col gap-4 p-6 md:px-16 md:flex-row justify-between items-start md:items-center w-full sticky top-0 bg-white/90 backdrop-blur-lg z-10"
    >
      <RouterLink to="/" class="button flex gap-2 items-center">
        <ArrowLeft />
      </RouterLink>
      <a v-if="!isMobileView" :href="projectData.link" target="_blank" class="flex-shrink-0 button">
        View Project <ExternalLink class="inline-block" size="20" />
      </a>
    </header>
    <main class="flex flex-col items-center w-full m-auto pt-8">
      <article class="grid gap-y-12 gap-x-0 pb-24 flex-1 w-full article-body">
        <div class="flex flex-col gap-4 full-bleed px-6 md:px-0">
          <h1 class="w-full text-2xl md:text-6xl font-bold text-gray-800">
            {{ projectData.title }}
          </h1>
          <p class="text-lg text-gray-600">{{ projectData.subtitle }}</p>
          <div class="flex flex-wrap gap-2">
            <span
              v-for="tag in projectData.tags"
              :key="tag"
              class="bg-gray-100 text-gray-700 font-medium px-2 py-1 rounded-md"
            >
              {{ tag }}
            </span>
          </div>
        </div>
        <img
          :src="projectData.heroImage"
          :alt="projectData.title"
          @click="openLightbox(projectData.heroImage)"
          class="w-full mb-8 md:rounded-2xl flex-1 flex-shrink object-cover full-bleed cursor-pointer"
        />

        <section
          v-for="(section, index) in projectData.sections"
          :key="index"
          class="flex flex-col gap-4 text-lg"
        >
          <h2 class="text-2xl font-bold text-gray-800">{{ section.title }}</h2>
          <p v-if="section.description" class="text-gray-600 -mt-2 leading-relaxed">
            {{ section.description }}
          </p>

          <div v-if="section.type === 'intro'">
            <p
              v-for="(p, i) in section.content"
              :key="i"
              class="text-gray-700 leading-relaxed mb-4 text-lg"
            >
              {{ p }}
            </p>
            <div v-if="section.role" class="mt-6">
              <h3 class="font-semibold text-gray-00 text-lg">{{ section.role.title }}</h3>
              <p v-if="section.role.description" class="text-gray-600 text-lg mb-2">
                {{ section.role.description }}
              </p>
              <ul class="flex flex-col gap-2 list-disc ml-6 text-gray-700 space-y-1">
                <li v-for="item in section.role.contributions" :key="item.title">
                  <strong>{{ item.title }}:</strong> {{ item.description }}
                </li>
              </ul>
            </div>
          </div>

          <div v-if="section.type === 'process'" class="flex flex-col gap-8 mt-2">
            <div v-for="phase in section.phases" :key="phase.title" class="flex flex-col">
              <h3 class="font-semibold text-gray-700 mb-1">{{ phase.title }}</h3>
              <p class="text-gray-700 leading-relaxed">{{ phase.description }}</p>
              <img
                v-if="phase.media"
                :src="phase.media.src"
                :alt="phase.media.alt"
                @click="openLightbox(phase.media.src)"
                class="image mt-4"
              />
            </div>
          </div>

          <div v-if="section.type === 'gallery'" class="grid gallery-section gap-4 mt-2">
            <figure v-for="(image, img_index) in section.images" :key="img_index">
              <img
                :src="image.src"
                :alt="image.caption"
                @click="openLightbox(image.src)"
                class="image"
              />
              <figcaption class="text-sm text-center text-gray-500 mt-2">
                {{ image.caption }}
              </figcaption>
            </figure>
          </div>

          <div v-if="section.type === 'impact'">
            <ul class="flex flex-col gap-2 list-disc ml-6 text-gray-700 space-y-1">
              <li v-for="(highlight, i) in section.highlights" :key="i">{{ highlight }}</li>
            </ul>
            <img
              v-if="section.media"
              :src="section.media.src"
              :alt="section.media.alt"
              @click="openLightbox(section.media.src)"
              class="image"
            />
          </div>

          <div v-if="section.type === 'learnings'">
            <ul class="flex flex-col gap-2 list-disc ml-6 text-gray-700 space-y-1">
              <li v-for="(point, i) in section.points" :key="i" class="">{{ point }}</li>
            </ul>
          </div>
        </section>
      </article>

      <a
        v-if="isMobileView"
        :href="projectData.link"
        target="_blank"
        class="fixed bottom-0 m-6 button"
      >
        View Project <ExternalLink class="inline-block" size="20" />
      </a>
    </main>
  </div>

  <div v-else class="flex flex-col items-center justify-center h-screen gap-4">
    <p class="text-lg text-red-500">Project Not Found</p>
    <p class="text-lg text-gray-600">
      A project with ID '{{ route.params.id }}' could not be found in your data file.
    </p>
    <RouterLink to="/" class="button flex gap-2 items-center mt-4">
      <ArrowLeft size="16" /> Go Back Home
    </RouterLink>
  </div>

  <Transition name="fade">
    <div
      v-if="lightboxImage"
      @click="closeLightbox"
      class="fixed inset-0 z-50 flex items-center justify-center overflow-auto bg-black/80 backdrop-blur-sm overscroll-contain"
    >
      <div class="fixed inset-0 bg-black/70 z-0" @click="closeLightbox"></div>
      <button
        @click.stop="closeLightbox"
        class="fixed top-0 right-0 m-4 p-4 z-20 text-white hover:bg-gray-900/60 rounded-2xl cursor-pointer active:scale-95 transition"
      >
        <X size="32" />
      </button>
      <img
        :src="lightboxImage"
        alt="Fullscreen image"
        @click.stop
        class="object-contain h-full rounded-lg z-10 p-6"
      />
    </div>
  </Transition>
</template>

<style scoped>
@reference "../assets/main.css";
.article-body {
  grid-template-columns: 1.5rem 1fr 1.5rem;
}
.article-body > *:not(.full-bleed) {
  grid-column: 2;
}
.full-bleed {
  grid-column: 1 / -1;
  max-width: 64rem;
  margin: auto;
}
.gallery-section {
  grid-template-columns: repeat(auto-fit, minmax(16rem, 1fr));
}
.image {
  @apply w-full rounded-2xl cursor-pointer border-1 border-gray-400
    hover:border-gray-600 hover:shadow-2xl transition duration-200 ease-in-out;
}

@media screen and (min-width: 768px) {
  .article-body {
    grid-template-columns: 1fr 40rem 1fr;
  }
}

/* Vue Transition for Lightbox */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
