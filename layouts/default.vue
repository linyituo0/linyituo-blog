<template>
  <div class="min-h-screen flex flex-col bg-gray-50 dark:bg-gray-900">
    <header class="bg-white dark:bg-gray-800 shadow-md">
      <div class="container mx-auto px-4 py-4">
        <div class="flex justify-between items-center">
          <NuxtLink to="/" class="text-2xl font-bold text-indigo-600 dark:text-indigo-400">
            个人博客
          </NuxtLink>
          <nav class="hidden md:flex space-x-6">
            <NuxtLink to="/" class="text-gray-700 dark:text-gray-300 hover:text-indigo-600 dark:hover:text-indigo-400">
              首页
            </NuxtLink>
            <NuxtLink to="/photos" class="text-gray-700 dark:text-gray-300 hover:text-indigo-600 dark:hover:text-indigo-400">
              照片墙
            </NuxtLink>
            <NuxtLink to="/about" class="text-gray-700 dark:text-gray-300 hover:text-indigo-600 dark:hover:text-indigo-400">
              关于我
            </NuxtLink>
            <NuxtLink to="/music" class="text-gray-700 dark:text-gray-300 hover:text-indigo-600 dark:hover:text-indigo-400">
              音乐分享
            </NuxtLink>
            <NuxtLink to="/movies" class="text-gray-700 dark:text-gray-300 hover:text-indigo-600 dark:hover:text-indigo-400">
              电影分享
            </NuxtLink>
          </nav>
          <div class="flex items-center space-x-4">
            <button @click="toggleColorMode" class="p-2 rounded-full hover:bg-gray-100 dark:hover:bg-gray-700">
              <Icon v-if="colorMode.value === 'dark'" name="ph:sun" class="w-5 h-5 text-yellow-400" />
              <Icon v-else name="ph:moon" class="w-5 h-5 text-gray-700" />
            </button>
            <button class="md:hidden p-2" @click="isMenuOpen = !isMenuOpen">
              <Icon name="heroicons:bars-3" class="w-6 h-6 text-gray-700 dark:text-gray-300" />
            </button>
          </div>
        </div>
      </div>
    </header>

    <!-- 移动端菜单 -->
    <div v-if="isMenuOpen" class="md:hidden bg-white dark:bg-gray-800 shadow-md">
      <div class="container mx-auto px-4 py-2">
        <nav class="flex flex-col space-y-3 py-3">
          <NuxtLink to="/" class="text-gray-700 dark:text-gray-300 hover:text-indigo-600 dark:hover:text-indigo-400 py-2">
            首页
          </NuxtLink>
          <NuxtLink to="/photos" class="text-gray-700 dark:text-gray-300 hover:text-indigo-600 dark:hover:text-indigo-400 py-2">
            照片墙
          </NuxtLink>
          <NuxtLink to="/about" class="text-gray-700 dark:text-gray-300 hover:text-indigo-600 dark:hover:text-indigo-400 py-2">
            关于我
          </NuxtLink>
          <NuxtLink to="/music" class="text-gray-700 dark:text-gray-300 hover:text-indigo-600 dark:hover:text-indigo-400 py-2">
            音乐分享
          </NuxtLink>
          <NuxtLink to="/movies" class="text-gray-700 dark:text-gray-300 hover:text-indigo-600 dark:hover:text-indigo-400 py-2">
            电影分享
          </NuxtLink>
        </nav>
      </div>
    </div>

    <main class="flex-grow container mx-auto px-4 py-8">
      <slot />
    </main>

    <footer class="bg-white dark:bg-gray-800 shadow-inner">
      <div class="container mx-auto px-4 py-6">
        <div class="text-center text-gray-600 dark:text-gray-400">
          <p>© {{ new Date().getFullYear() }} 个人博客. 保留所有权利.</p>
          <p class="mt-2 text-sm">使用 Nuxt 3 和 TypeScript 构建</p>
        </div>
      </div>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { useColorMode } from '@nuxtjs/color-mode';

const colorMode = useColorMode();
const isMenuOpen = ref(false);

const toggleColorMode = () => {
  colorMode.preference = colorMode.value === 'dark' ? 'light' : 'dark';
};
</script>

<style scoped>
.router-link-active {
  @apply text-indigo-600 dark:text-indigo-400 font-medium;
}
</style> 