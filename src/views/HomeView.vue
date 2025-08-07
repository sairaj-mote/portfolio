<script setup>
import { ref, computed } from 'vue'
import { RouterLink, useRouter } from 'vue-router'
import HeroBlockText from '../components/HeroBlockText.vue'
import FooterThankYou from '@/components/FooterThankYou.vue'
import ProjectCard from '../components/ProjectCard.vue'
import { Check, Mail, ArrowDown } from 'lucide-vue-next'
import projects from '../data/projects.json'

const props = defineProps({
  filter: {
    type: String,
    default: 'all',
  },
})

const router = useRouter()

let mailCopied = ref(false)
function copyEmail() {
  mailCopied.value = true
  navigator.clipboard.writeText('sairajmote3@gmail.com')
  setTimeout(() => {
    mailCopied.value = false
  }, 2000)
}

const projectTypes = [
  { name: 'All', value: 'all' },
  { name: 'Web App', value: 'web-app' },
  { name: 'Ecommerce', value: 'ecommerce' },
  { name: 'Brand Site', value: 'brand-site' },
  { name: 'Library', value: 'library' },
]

const filteredProjects = computed(() => {
  if (props.filter === 'all') {
    return projects.filter((p) => !p.ignore)
  }
  return projects.filter((p) => !p.ignore && p.type === props.filter)
})

function setFilter(type) {
  router.push({
    hash: '#projects',
    query: { filter: type === 'all' ? undefined : type },
  })
}

function handleKeydown(e) {
  if (e.key === 'Enter' || e.key === ' ') {
    e.preventDefault()
    e.target.click()
  }
}
</script>

<template>
  <header class="w-full sticky top-0 bg-white/90 backdrop-blur-lg z-10">
    <div class="flex gap-2 p-4 max-w-[64rem] w-full m-auto">
      <a
        href="https://github.com/sairaj-mote"
        target="_blank"
        class="opacity-70 hover:opacity-100 active:scale-95 transition duration-200 ease-in-out"
      >
        <span class="sr-only">GitHub</span>
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 98 96" class="w-6 h-6">
          <path
            fill-rule="evenodd"
            clip-rule="evenodd"
            d="M48.854 0C21.839 0 0 22 0 49.217c0 21.756 13.993 40.172 33.405 46.69 2.427.49 3.316-1.059 3.316-2.362 0-1.141-.08-5.052-.08-9.127-13.59 2.934-16.42-5.867-16.42-5.867-2.184-5.704-5.42-7.17-5.42-7.17-4.448-3.015.324-3.015.324-3.015 4.934.326 7.523 5.052 7.523 5.052 4.367 7.496 11.404 5.378 14.235 4.074.404-3.178 1.699-5.378 3.074-6.6-10.839-1.141-22.243-5.378-22.243-24.283 0-5.378 1.94-9.778 5.014-13.2-.485-1.222-2.184-6.275.486-13.038 0 0 4.125-1.304 13.426 5.052a46.97 46.97 0 0 1 12.214-1.63c4.125 0 8.33.571 12.213 1.63 9.302-6.356 13.427-5.052 13.427-5.052 2.67 6.763.97 11.816.485 13.038 3.155 3.422 5.015 7.822 5.015 13.2 0 18.905-11.404 23.06-22.324 24.283 1.78 1.548 3.316 4.481 3.316 9.126 0 6.6-.08 11.897-.08 13.526 0 1.304.89 2.853 3.316 2.364 19.412-6.52 33.405-24.935 33.405-46.691C97.707 22 75.788 0 48.854 0z"
            fill="#000"
          />
        </svg>
      </a>
      <a
        href="https://www.linkedin.com/in/sairaj-mote-6605767a/"
        target="_blank"
        class="opacity-70 hover:opacity-100 active:scale-95 transition duration-200 ease-in-out"
      >
        <span class="sr-only">LinkedIn</span>
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 72 72" class="w-6 h-6">
          <g fill="none" fill-rule="evenodd">
            <path
              d="M8,72 L64,72 C68.418278,72 72,68.418278 72,64 L72,8 C72,3.581722 68.418278,-8.11624501e-16 64,0 L8,0 C3.581722,8.11624501e-16 -5.41083001e-16,3.581722 0,8 L0,64 C5.41083001e-16,68.418278 3.581722,72 8,72 Z"
              fill="#000"
            />
            <path
              d="M62,62 L51.315625,62 L51.315625,43.8021149 C51.315625,38.8127542 49.4197917,36.0245323 45.4707031,36.0245323 C41.1746094,36.0245323 38.9300781,38.9261103 38.9300781,43.8021149 L38.9300781,62 L28.6333333,62 L28.6333333,27.3333333 L38.9300781,27.3333333 L38.9300781,32.0029283 C38.9300781,32.0029283 42.0260417,26.2742151 49.3825521,26.2742151 C56.7356771,26.2742151 62,30.7644705 62,40.051212 L62,62 Z M16.349349,22.7940133 C12.8420573,22.7940133 10,19.9296567 10,16.3970067 C10,12.8643566 12.8420573,10 16.349349,10 C19.8566406,10 22.6970052,12.8643566 22.6970052,16.3970067 C22.6970052,19.9296567 19.8566406,22.7940133 16.349349,22.7940133 Z M11.0325521,62 L21.769401,62 L21.769401,27.3333333 L11.0325521,27.3333333 L11.0325521,62 Z"
              fill="#FFF"
            />
          </g>
        </svg>
      </a>
      <a
        href="mailto:sairajmote3@gmail"
        target="_blank"
        class="ml-auto opacity-70 hover:opacity-100 active:scale-95 transition duration-200 ease-in-out"
      >
        sairajmote3@gmail.com
      </a>
    </div>
  </header>
  <section class="flex flex-col justify-center items-center min-h-screen">
    <div
      class="flex flex-col md:grid md:grid-cols-[18rem_1fr] gap-16 justify-center m-auto max-w-[64rem] px-6 pt-6 md:p-8 h-full"
    >
      <div class="flex flex-col w-full gap-8 h-[calc(100vh-7rem)] md:h-auto">
        <img
          src="/me.webp"
          class="aspect-square object-cover rounded-2xl max-w-[16rem]"
          alt="Sairaj Mote Profile Picture"
        />
        <h1 class="text-2xl font-bold">
          Hi there! <br />
          I'm Sairaj Mote
        </h1>
        <div class="h-full flex-1 md:h-48 w-[1px] bg-black"></div>
        <div class="flex flex-col gap-2 items-start">
          <RouterLink to="#projects" class="button flex gap-2 items-center">
            <ArrowDown />
            Check out my work</RouterLink
          >
          <button class="button flex gap-2 items-center" @click="copyEmail">
            <span v-if="!mailCopied" class="flex gap-2 items-center">
              <Mail />
              Copy my email</span
            ><span v-else class="flex gap-2 items-center"
              >copied
              <Check />
            </span>
          </button>
        </div>
      </div>
      <div class="flex flex-col flex-grow gap-12 max-w-[30rem]">
        <HeroBlockText />
        <h2 class="font-medium text-gray-700 leading-8 text-xl">
          With a passion for clean design and clean code, I craft user interfaces that not only look
          great but feel just right. Whether I’m designing in Figma, coding in Vue or React, or
          bringing visuals to life with Vanilla JavaScript, I’m all about creating seamless, joyful
          experiences that people actually enjoy using.
        </h2>
      </div>
    </div>
  </section>
  <section id="projects" class="px-6 md:px-8 py-16">
    <div class="flex gap-8 flex-col max-w-[72rem] m-auto">
      <div class="flex flex-col gap-4">
        <h1 class="text-6xl font-bold uppercase">Projects</h1>
        <fieldset
          class="flex gap-2 flex-wrap"
          @change="setFilter($event.target.value)"
          @keydown="handleKeydown"
        >
          <label
            class="flex group"
            v-for="projectType in projectTypes"
            :key="projectType.value"
            tabindex="0"
            :for="projectType.value"
          >
            <input
              type="radio"
              name="project-type"
              :id="projectType.value"
              :value="projectType.value"
              class="hidden"
              :checked="projectType.value === filter"
            />
            <span
              class="flex-shrink-0 px-4 py-2 uppercase tracking-wider rounded-lg cursor-pointer active:scale-95 bg-gray-200 hover:bg-black hover:text-white transition duration-200 ease-in-out group-has-[:checked]:bg-black group-has-[:checked]:text-white"
              >{{ projectType.name }}</span
            >
          </label>
        </fieldset>
      </div>
      <ul class="flex flex-col md:gap-8 gap-y-16">
        <ProjectCard v-for="project in filteredProjects" :key="project.name" v-bind="project">
        </ProjectCard>
      </ul>
    </div>
  </section>
  <footer class="flex flex-col gap-4 items-center py-16 max-w-[72rem] m-auto px-6">
    <FooterThankYou />
    <p class="text-sm text-gray-700">&copy; {{ new Date().getFullYear() }} Sairaj Mote</p>
  </footer>
</template>

<style>
body {
  background: white;
  background-image: radial-gradient(#ccc 1px, transparent 0);
  background-size: 40px 40px;
  background-position: -19px -19px;
}
@reference '../assets/main.css';
.button {
  @apply bg-black py-2 px-4 rounded-lg tracking-wider
   text-white font-medium hover:bg-white hover:text-black border-black border-2 
   active:scale-95
   transition duration-200 ease-in-out uppercase cursor-pointer;
}
</style>
