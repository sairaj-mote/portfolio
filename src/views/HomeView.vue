<script setup>
import { ref } from 'vue'
import { RouterLink } from 'vue-router'
import HeroBlockText from '../components/HeroBlockText.vue'
import ProjectCard from '../components/ProjectCard.vue'
import { Check, Mail, ArrowDown } from 'lucide-vue-next'
import projects from '../data/projects.json'

let mailCopied = ref(false)
function copyEmail() {
  mailCopied.value = true
  navigator.clipboard.writeText('sairajmote3@gmail.com')
  setTimeout(() => {
    mailCopied.value = false
  }, 2000)
}
</script>
<template>
  <section class="flex justify-center items-center min-h-screen">
    <div class="grid md:grid-cols-[18rem_1fr] gap-16 justify-center m-auto max-w-[64rem] p-8">
      <div class="flex flex-col w-full gap-8">
        <img
          src="../assets/sairaj-mote.jpg"
          class="aspect-square object-cover rounded-2xl"
          alt="Sairaj Mote Profile Picture"
        />
        <h1 class="text-2xl font-bold">
          Hi there! <br />
          I'm Sairaj Mote
        </h1>
        <div class="h-48 w-[1px] bg-black"></div>
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
      <div class="flex flex-col flex-grow bg-white gap-12 max-w-[30rem]">
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
  <section id="projects" class="px-8 py-16">
    <div class="columns-1 md:columns-2 gap-4 flex-col max-w-[64rem] m-auto">
      <h1 class="text-6xl font-bold uppercase">Projects</h1>
      <ul class="gap-4">
        <li class="p-8 h-28 break-inside-avoid"></li>
        <ProjectCard v-for="project in projects" :key="project.name" v-bind="project">
        </ProjectCard>
      </ul>
    </div>
  </section>
</template>

<style>
@reference "../assets/main.css";
.button {
  @apply bg-black py-2 px-4 rounded-lg tracking-wider text-white font-medium hover:bg-white hover:text-black border-black border-2 transition-colors duration-200 ease-in-out uppercase cursor-pointer;
}
</style>
