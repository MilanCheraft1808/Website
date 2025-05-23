﻿<script setup lang="ts">
import {ref, computed} from 'vue'
import {useRoute} from 'vue-router'

const navItems = [
  {label: '_Hello', to: '/'},
  {label: '_about-me', to: '/about'},
  {label: '_projects', to: '/projects'},
]
const route = useRoute()
const currentPath = computed(() => route.path)

const isOpen = ref(false)
</script>

<template>
  <nav class="w-full bg-slate-900 text-slate-400 relative">
    <div class="mx-auto flex border-1 border-b-slate-400 items-center">
      <div class="text-body-md pl-2">Milan Chéraft</div>
      <div class="ml-32">
        <ul class="hidden md:flex border-x divide-x divide-slate-400 rounded-t-lg overflow-hidden">
          <li v-for="item in navItems" :key="item.to">
            <NuxtLink
                :to="item.to"
                class="block px-4 py-2 border-b-2 transition-colors"
                :class="currentPath === item.to
                ? 'border-yellow-500 text-slate-50'
                : 'border-transparent text-slate-400 hover:text-slate-50 hover:border-gray-700'"
            >
              {{ item.label }}
            </NuxtLink>
          </li>
        </ul>
      </div>
      <div class="ml-auto flex items-center">
        <NuxtLink
            to="/contact"
            class="hidden pl-2 pr-2 md:block text-body-md md:text-base py-2 border-b-2 border-slate-400 border-l-solid border-l-1 border-l-slate-400 transition-colors"
            :class="currentPath === '/contact'
            ? 'border-b-yellow-500 text-slate-50'
                : 'border-transparent  text-slate-400 hover:text-slate-50 hover:border-gray-700'"
        >
          _contact-me
        </NuxtLink>

        <button
            class="md:hidden text-slate-400 hover:text-slate-50 transition-colors p-2"
            @click="isOpen = true"
        >
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24"
               viewBox="0 0 24 24" fill="none" stroke="currentColor"
               stroke-width="2" stroke-linecap="round" stroke-linejoin="round"
               class="lucide lucide-menu">
            <line x1="4" y1="6" x2="20" y2="6"/>
            <line x1="4" y1="12" x2="20" y2="12"/>
            <line x1="4" y1="18" x2="20" y2="18"/>
          </svg>
        </button>
      </div>
    </div>

    <transition name="fade">
      <div
          v-if="isOpen"
          class="fixed inset-0 z-50 bg-slate-900 text-slate-400 flex flex-col p-4"
      >
        <div class="flex items-center justify-between mb-8">
          <div class="text-body-md pl-2">Milan Chéraft</div>
          <button @click="isOpen = false">
            &times;
          </button>
        </div>
        <nav class="flex-grow">
          <ul class="border-t border-b border-b-slate-400 divide-y divide-gray-700 rounded-lg overflow-hidden">
            <li
                v-for="item in [...navItems, { label: '_contact-me', to: '/contact' }]"
                :key="item.to"
            >
              <NuxtLink
                  :to="item.to"
                  @click.native="isOpen = false"
                  class="block px-4 py-3 border-b-2 transition-colors"
                  :class="currentPath === item.to
                  ? 'border-yellow-500 text-slate-50'
                  : 'border-transparent text-slate-400 hover:text-slate-50 hover:border-gray-700'"
              >
                {{ item.label }}
              </NuxtLink>
            </li>
          </ul>
        </nav>
      </div>
    </transition>

  </nav>
</template>

<style>
.fade-enter-active, .fade-leave-active {
  transition: opacity .4s
}

.fade-enter-from, .fade-leave-to {
  opacity: 0
}
</style>
