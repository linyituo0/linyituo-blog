<template>
  <div>
    <div class="mb-8">
      <h1 class="text-3xl font-bold text-gray-800 dark:text-white mb-4">照片墙</h1>
      <p class="text-gray-600 dark:text-gray-400">这里展示了我拍摄的一些照片，记录生活中的美好瞬间。</p>
    </div>

    <div class="mb-8">
      <div class="flex flex-wrap gap-4">
        <button 
          v-for="category in categories" 
          :key="category.id"
          @click="selectedCategory = category.id"
          :class="[
            'px-4 py-2 rounded-full text-sm font-medium transition-colors',
            selectedCategory === category.id 
              ? 'bg-indigo-600 text-white' 
              : 'bg-gray-100 dark:bg-gray-700 text-gray-700 dark:text-gray-300 hover:bg-gray-200 dark:hover:bg-gray-600'
          ]"
        >
          {{ category.name }}
        </button>
      </div>
    </div>

    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
      <div 
        v-for="photo in filteredPhotos" 
        :key="photo.id" 
        class="group relative aspect-square bg-gray-200 dark:bg-gray-700 rounded-lg overflow-hidden cursor-pointer"
        @click="openPhotoModal(photo)"
      >
        <div class="absolute inset-0 flex items-center justify-center">
          <p class="text-gray-500 dark:text-gray-400">{{ photo.title }}</p>
        </div>
        <div class="absolute inset-0 bg-black bg-opacity-0 group-hover:bg-opacity-30 transition-all duration-300 flex items-center justify-center opacity-0 group-hover:opacity-100">
          <div class="text-white text-center p-4">
            <h3 class="font-medium">{{ photo.title }}</h3>
            <p class="text-sm mt-1">{{ photo.date }}</p>
          </div>
        </div>
      </div>
    </div>

    <!-- 照片模态框 -->
    <div v-if="selectedPhoto" class="fixed inset-0 bg-black bg-opacity-75 z-50 flex items-center justify-center p-4">
      <div class="max-w-4xl w-full bg-white dark:bg-gray-800 rounded-lg overflow-hidden shadow-xl">
        <div class="relative">
          <div class="aspect-video bg-gray-200 dark:bg-gray-700 flex items-center justify-center">
            <p class="text-gray-500 dark:text-gray-400">{{ selectedPhoto.title }}</p>
          </div>
          <button @click="selectedPhoto = null" class="absolute top-4 right-4 bg-black bg-opacity-50 text-white rounded-full p-2 hover:bg-opacity-70">
            <Icon name="ph:x" class="w-5 h-5" />
          </button>
        </div>
        <div class="p-6">
          <h3 class="text-xl font-bold text-gray-800 dark:text-white">{{ selectedPhoto.title }}</h3>
          <p class="text-gray-600 dark:text-gray-400 mt-2">{{ selectedPhoto.description }}</p>
          <div class="mt-4 flex items-center text-sm text-gray-500 dark:text-gray-400">
            <Icon name="ph:calendar" class="w-4 h-4 mr-1" />
            <span>{{ selectedPhoto.date }}</span>
            <span class="mx-2">•</span>
            <Icon name="ph:tag" class="w-4 h-4 mr-1" />
            <span>{{ getCategoryName(selectedPhoto.categoryId) }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';

definePageMeta({
  layout: 'default'
});

interface Category {
  id: number;
  name: string;
}

interface Photo {
  id: number;
  title: string;
  description: string;
  date: string;
  categoryId: number;
}

// 照片分类
const categories = ref<Category[]>([
  { id: 0, name: '全部' },
  { id: 1, name: '风景' },
  { id: 2, name: '城市' },
  { id: 3, name: '人物' },
  { id: 4, name: '美食' }
]);

// 照片数据
const photos = ref<Photo[]>([
  { 
    id: 1, 
    title: '山间日出', 
    description: '清晨在山顶拍摄的日出，金色的阳光洒在云海上，美不胜收。', 
    date: '2023-05-15', 
    categoryId: 1 
  },
  { 
    id: 2, 
    title: '城市夜景', 
    description: '繁华都市的夜晚，霓虹闪烁，车水马龙。', 
    date: '2023-06-22', 
    categoryId: 2 
  },
  { 
    id: 3, 
    title: '海边漫步', 
    description: '傍晚时分在海边散步，欣赏落日余晖。', 
    date: '2023-07-10', 
    categoryId: 1 
  },
  { 
    id: 4, 
    title: '街头艺人', 
    description: '城市街头的艺术表演者，用音乐点亮城市的一角。', 
    date: '2023-08-05', 
    categoryId: 3 
  },
  { 
    id: 5, 
    title: '美味早餐', 
    description: '精心准备的健康早餐，开启美好的一天。', 
    date: '2023-09-18', 
    categoryId: 4 
  },
  { 
    id: 6, 
    title: '古镇小巷', 
    description: '雨后的古镇小巷，青石板路上倒映着古老的建筑。', 
    date: '2023-10-03', 
    categoryId: 2 
  },
  { 
    id: 7, 
    title: '花海', 
    description: '春天里盛开的花海，色彩斑斓，生机勃勃。', 
    date: '2023-04-12', 
    categoryId: 1 
  },
  { 
    id: 8, 
    title: '咖啡时光', 
    description: '安静的下午，一杯香浓的咖啡，一本好书。', 
    date: '2023-11-20', 
    categoryId: 4 
  }
]);

const selectedCategory = ref(0);
const selectedPhoto = ref<Photo | null>(null);

// 根据分类筛选照片
const filteredPhotos = computed(() => {
  if (selectedCategory.value === 0) {
    return photos.value;
  }
  return photos.value.filter(photo => photo.categoryId === selectedCategory.value);
});

// 获取分类名称
const getCategoryName = (categoryId: number): string => {
  const category = categories.value.find(c => c.id === categoryId);
  return category ? category.name : '';
};

// 打开照片详情模态框
const openPhotoModal = (photo: Photo) => {
  selectedPhoto.value = photo;
};
</script> 