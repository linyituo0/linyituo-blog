<template>
  <div>
    <div class="mb-8">
      <h1 class="text-3xl font-bold text-gray-800 dark:text-white mb-4">关于我</h1>
      <div class="h-1 w-20 bg-indigo-600 rounded"></div>
    </div>

    <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
      <div class="md:col-span-1">
        <div class="bg-white dark:bg-gray-800 rounded-lg shadow-md overflow-hidden">
          <div class="aspect-square bg-gray-200 dark:bg-gray-700 flex items-center justify-center relative">
            <img src="/images/m2.jpeg" alt="个人照片" class="w-full h-full object-cover" />
            
            <div class="absolute top-0 right-0 w-16 h-16">
              <HeartCanvas />
            </div>
          </div>
          <div class="p-6">
            <h2 class="text-xl font-bold text-gray-800 dark:text-white mb-2">林一拓</h2>
            <p class="text-gray-600 dark:text-gray-400 mb-4">网站开发者 / 摄影爱好者</p>
            
            <div class="flex space-x-4 mt-6">
              <a href="#" class="text-gray-600 dark:text-gray-400 hover:text-indigo-600 dark:hover:text-indigo-400">
                <Icon name="mdi:github" class="w-6 h-6" />
              </a>
              <a href="#" class="text-gray-600 dark:text-gray-400 hover:text-indigo-600 dark:hover:text-indigo-400">
                <Icon name="mdi:twitter" class="w-6 h-6" />
              </a>
              <a href="#" class="text-gray-600 dark:text-gray-400 hover:text-indigo-600 dark:hover:text-indigo-400">
                <Icon name="mdi:instagram" class="w-6 h-6" />
              </a>
              <a href="#" class="text-gray-600 dark:text-gray-400 hover:text-indigo-600 dark:hover:text-indigo-400">
                <Icon name="mdi:email" class="w-6 h-6" />
              </a>
            </div>
          </div>
        </div>
      </div>

      <div class="md:col-span-2">
        <div class="bg-white dark:bg-gray-800 rounded-lg shadow-md p-6 mb-8">
          <h2 class="text-xl font-bold text-gray-800 dark:text-white mb-4">个人简介</h2>
          <p class="text-gray-600 dark:text-gray-400 mb-4">
            大家好，我是林一坨，一名热爱生活、热爱创作的网站开发者。我喜欢通过摄影记录生活中的美好瞬间，通过音乐和电影感受不同的情感和故事。
          </p>
          <p class="text-gray-600 dark:text-gray-400 mb-4">
            我创建这个个人博客是为了分享我的兴趣爱好和生活点滴，希望能与志同道合的朋友们交流。在这里，你可以看到我拍摄的照片、我喜欢的音乐和电影，以及了解更多关于我的信息。
          </p>
          <p class="text-gray-600 dark:text-gray-400">
            如果你有任何问题或想法，欢迎通过社交媒体或邮件与我联系，我很期待与你交流！
          </p>
        </div>

        <div class="bg-white dark:bg-gray-800 rounded-lg shadow-md p-6 mb-8">
          <h2 class="text-xl font-bold text-gray-800 dark:text-white mb-4">兴趣爱好</h2>
          <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
            <div v-for="(hobby, index) in hobbies" :key="index" class="flex items-start">
              <div class="bg-indigo-100 dark:bg-indigo-900 p-2 rounded-lg mr-3">
                <Icon :name="hobby.icon" class="w-5 h-5 text-indigo-600 dark:text-indigo-400" />
              </div>
              <div>
                <h3 class="font-medium text-gray-800 dark:text-white">{{ hobby.name }}</h3>
                <p class="text-sm text-gray-600 dark:text-gray-400 mt-1">{{ hobby.description }}</p>
              </div>
            </div>
          </div>
        </div>

        <div class="bg-white dark:bg-gray-800 rounded-lg shadow-md p-6">
          <h2 class="text-xl font-bold text-gray-800 dark:text-white mb-4">技能</h2>
          <div class="space-y-4">
            <div v-for="(skill, index) in skills" :key="index">
              <div class="flex justify-between mb-1">
                <span class="text-gray-700 dark:text-gray-300 font-medium">{{ skill.name }}</span>
                <span class="text-gray-600 dark:text-gray-400 text-sm">{{ skill.level }}%</span>
              </div>
              <div class="w-full bg-gray-200 dark:bg-gray-700 rounded-full h-2">
                <div class="bg-indigo-600 h-2 rounded-full" :style="{ width: `${skill.level}%` }"></div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

definePageMeta({
  layout: 'default'
});

interface Hobby {
  name: string;
  description: string;
  icon: string;
}

interface Skill {
  name: string;
  level: number;
}

const hobbies = ref<Hobby[]>([
  {
    name: '摄影',
    description: '喜欢用相机记录生活中的美好瞬间，尤其喜欢风景和街头摄影。',
    icon: 'ph:camera'
  },
  {
    name: '音乐',
    description: '热爱各种类型的音乐，尤其是民谣和轻音乐，闲暇时喜欢弹吉他。',
    icon: 'ph:music-notes'
  },
  {
    name: '电影',
    description: '喜欢观看各种类型的电影，尤其是剧情片和纪录片。',
    icon: 'ph:film-slate'
  },
  {
    name: '阅读',
    description: '喜欢阅读各类书籍，尤其是科技、历史和哲学类书籍。',
    icon: 'ph:book-open'
  },
  {
    name: '旅行',
    description: '热爱探索不同的地方，体验不同的文化和风景。',
    icon: 'ph:airplane'
  },
  {
    name: '编程',
    description: '喜欢学习新的编程技术，开发有趣的项目。',
    icon: 'ph:code'
  }
]);

const skills = ref<Skill[]>([
  { name: '前端开发 (HTML, CSS, JavaScript)', level: 90 },
  { name: 'Vue.js / Nuxt.js', level: 85 },
  { name: 'TypeScript', level: 80 },
  { name: '响应式设计', level: 85 },
  { name: '摄影', level: 75 },
  { name: 'UI/UX 设计', level: 70 }
]);
</script> 