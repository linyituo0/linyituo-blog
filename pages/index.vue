<template>
  <div>
    <section class="mb-12">
      <div class="bg-gradient-to-r from-indigo-500 to-purple-600 rounded-xl shadow-lg overflow-hidden">
        <div class="md:flex">
          <div class="md:w-1/2 p-8 md:p-12 flex flex-col justify-center">
            <h1 class="text-3xl md:text-4xl font-bold text-white mb-4">欢迎来到林一坨的个人空间</h1>
            <p class="text-indigo-100 text-lg mb-6">这里记录着我的生活、兴趣爱好以及各种分享。</p>
            <div class="flex space-x-4">
              <NuxtLink to="/about" class="bg-white text-indigo-600 hover:bg-indigo-50 px-6 py-2 rounded-lg font-medium transition duration-200">
                了解更多
              </NuxtLink>
              <NuxtLink to="/photos" class="bg-indigo-700 text-white hover:bg-indigo-800 px-6 py-2 rounded-lg font-medium transition duration-200">
                浏览照片
              </NuxtLink>
            </div>
          </div>
          <div class="md:w-1/2 p-8 flex items-center justify-center">
            <div class="w-full max-w-md h-48 md:h-64 lg:h-80 bg-indigo-200 rounded-lg overflow-hidden">
              <img 
                src="/images/hanfu-girl.jpeg" 
                alt="封面图片" 
                class="w-full h-full object-cover" 
              />
            </div>
          </div>
        </div>
      </div>
    </section>

    <section class="mb-12">
      <h2 class="text-2xl font-bold text-gray-800 dark:text-white mb-6">最新照片</h2>
      <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
        <div v-for="i in 4" :key="i" class="aspect-square bg-gray-200 dark:bg-gray-700 rounded-lg flex items-center justify-center">
          <p class="text-gray-500 dark:text-gray-400">照片 {{ i }}</p>
        </div>
      </div>
      <div class="mt-6 text-center">
        <NuxtLink to="/photos" class="text-indigo-600 dark:text-indigo-400 hover:underline font-medium">
          查看更多照片 →
        </NuxtLink>
      </div>
    </section>

    <section class="mb-12">
      <h2 class="text-2xl font-bold text-gray-800 dark:text-white mb-6">音乐分享</h2>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <div v-for="i in 3" :key="i" class="bg-white dark:bg-gray-800 rounded-lg shadow-md overflow-hidden">
          <div class="h-40 bg-gray-200 dark:bg-gray-700 flex items-center justify-center">
            <p class="text-gray-500 dark:text-gray-400">音乐封面 {{ i }}</p>
          </div>
          <div class="p-4">
            <h3 class="font-bold text-gray-800 dark:text-white">音乐标题 {{ i }}</h3>
            <p class="text-gray-600 dark:text-gray-400 text-sm mt-1">歌手名称</p>
            <div class="mt-4 flex justify-between items-center">
              <span class="text-xs text-gray-500 dark:text-gray-400">流行 / 2023</span>
              <button class="text-indigo-600 dark:text-indigo-400 hover:text-indigo-700 dark:hover:text-indigo-300">
                <Icon name="ph:play-circle" class="w-8 h-8" />
              </button>
            </div>
          </div>
        </div>
      </div>
      <div class="mt-6 text-center">
        <NuxtLink to="/music" class="text-indigo-600 dark:text-indigo-400 hover:underline font-medium">
          查看更多音乐 →
        </NuxtLink>
      </div>
    </section>

    <section>
      <h2 class="text-2xl font-bold text-gray-800 dark:text-white mb-6">电影推荐</h2>
      <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <div v-for="i in 2" :key="i" class="bg-white dark:bg-gray-800 rounded-lg shadow-md overflow-hidden flex">
          <div class="w-1/3 bg-gray-200 dark:bg-gray-700 flex items-center justify-center">
            <p class="text-gray-500 dark:text-gray-400">电影海报 {{ i }}</p>
          </div>
          <div class="w-2/3 p-4">
            <h3 class="font-bold text-gray-800 dark:text-white">电影标题 {{ i }}</h3>
            <p class="text-gray-600 dark:text-gray-400 text-sm mt-1">导演 / 2023</p>
            <p class="text-gray-600 dark:text-gray-400 text-sm mt-3 line-clamp-3">
              这是一部精彩的电影，讲述了一个引人入胜的故事。这里是电影的简短描述，介绍电影的主要情节和亮点。
            </p>
            <div class="mt-3">
              <span class="inline-block bg-gray-100 dark:bg-gray-700 text-gray-600 dark:text-gray-300 text-xs px-2 py-1 rounded mr-2">
                剧情
              </span>
              <span class="inline-block bg-gray-100 dark:bg-gray-700 text-gray-600 dark:text-gray-300 text-xs px-2 py-1 rounded">
                冒险
              </span>
            </div>
          </div>
        </div>
      </div>
      <div class="mt-6 text-center">
        <NuxtLink to="/movies" class="text-indigo-600 dark:text-indigo-400 hover:underline font-medium">
          查看更多电影 →
        </NuxtLink>
      </div>
    </section>
  </div>
</template>

<script setup lang="ts">
definePageMeta({
  layout: 'default'
});
</script> 