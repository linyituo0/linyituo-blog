<template>
  <div>
    <div class="mb-8">
      <h1 class="text-3xl font-bold text-gray-800 dark:text-white mb-4">电影分享</h1>
      <p class="text-gray-600 dark:text-gray-400">这里分享我喜欢的电影，希望你也能喜欢。</p>
    </div>

    <div class="mb-8">
      <div class="flex flex-wrap gap-4">
        <button 
          v-for="genre in movieGenres" 
          :key="genre.id"
          @click="selectedGenre = genre.id"
          :class="[
            'px-4 py-2 rounded-full text-sm font-medium transition-colors',
            selectedGenre === genre.id 
              ? 'bg-indigo-600 text-white' 
              : 'bg-gray-100 dark:bg-gray-700 text-gray-700 dark:text-gray-300 hover:bg-gray-200 dark:hover:bg-gray-600'
          ]"
        >
          {{ genre.name }}
        </button>
      </div>
    </div>

    <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
      <div 
        v-for="movie in filteredMovies" 
        :key="movie.id" 
        class="bg-white dark:bg-gray-800 rounded-lg shadow-md overflow-hidden flex flex-col md:flex-row"
      >
        <div class="md:w-1/3 h-48 md:h-auto bg-gray-200 dark:bg-gray-700 flex items-center justify-center">
          <p class="text-gray-500 dark:text-gray-400">{{ movie.title }} - 海报</p>
        </div>
        <div class="md:w-2/3 p-6">
          <div class="flex justify-between items-start">
            <div>
              <h3 class="font-bold text-gray-800 dark:text-white text-xl">{{ movie.title }}</h3>
              <p class="text-gray-600 dark:text-gray-400 mt-1">{{ movie.director }} / {{ movie.year }}</p>
            </div>
            <div class="flex items-center">
              <span class="text-yellow-500 mr-1">
                <Icon name="ph:star-fill" class="w-5 h-5" />
              </span>
              <span class="font-bold text-gray-800 dark:text-white">{{ movie.rating }}</span>
              <span class="text-gray-500 dark:text-gray-400 text-sm">/10</span>
            </div>
          </div>
          
          <div class="mt-4 flex flex-wrap gap-2">
            <span 
              v-for="(tag, index) in movie.tags" 
              :key="index" 
              class="inline-block bg-gray-100 dark:bg-gray-700 text-gray-600 dark:text-gray-300 text-xs px-2 py-1 rounded"
            >
              {{ tag }}
            </span>
          </div>
          
          <p class="text-gray-600 dark:text-gray-400 text-sm mt-4">{{ movie.description }}</p>
          
          <div class="mt-6 flex justify-between items-center">
            <div class="flex space-x-3">
              <button @click="toggleFavorite(movie)" class="text-gray-500 dark:text-gray-400 hover:text-red-500 dark:hover:text-red-400">
                <Icon :name="movie.isFavorite ? 'ph:heart-fill' : 'ph:heart'" class="w-6 h-6" :class="{ 'text-red-500': movie.isFavorite }" />
              </button>
              <button @click="toggleWatched(movie)" class="text-gray-500 dark:text-gray-400 hover:text-green-500 dark:hover:text-green-400">
                <Icon :name="movie.isWatched ? 'ph:check-circle-fill' : 'ph:check-circle'" class="w-6 h-6" :class="{ 'text-green-500': movie.isWatched }" />
              </button>
            </div>
            <a :href="movie.link" target="_blank" class="bg-indigo-600 hover:bg-indigo-700 text-white px-4 py-2 rounded-lg text-sm font-medium transition duration-200">
              了解更多
            </a>
          </div>
        </div>
      </div>
    </div>

    <div v-if="filteredMovies.length === 0" class="text-center py-12">
      <Icon name="ph:film-slash" class="w-16 h-16 text-gray-400 dark:text-gray-600 mx-auto mb-4" />
      <h3 class="text-xl font-medium text-gray-700 dark:text-gray-300">没有找到相关电影</h3>
      <p class="text-gray-500 dark:text-gray-400 mt-2">请尝试选择其他类型</p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';

definePageMeta({
  layout: 'default'
});

interface MovieGenre {
  id: number;
  name: string;
}

interface Movie {
  id: number;
  title: string;
  director: string;
  year: string;
  rating: number;
  genreId: number;
  tags: string[];
  description: string;
  link: string;
  isFavorite: boolean;
  isWatched: boolean;
}

// 电影类型
const movieGenres = ref<MovieGenre[]>([
  { id: 0, name: '全部' },
  { id: 1, name: '剧情' },
  { id: 2, name: '科幻' },
  { id: 3, name: '动作' },
  { id: 4, name: '喜剧' },
  { id: 5, name: '动画' }
]);

// 电影数据
const movies = ref<Movie[]>([
  { 
    id: 1, 
    title: '哪吒-魔童降世', 
    director: '饺子', 
    year: '2019', 
    rating: 9.7, 
    genreId: 1, 
    tags: ['剧情', '动画', '奇幻'], 
    description: '哪吒与命运抗争，最终成为拯救世界的英雄。', 
    link: 'https://movie.douban.com/subject/30110912/',
    isFavorite: true,
    isWatched: true
  },
  { 
    id: 2, 
    title: '哪吒-魔童闹海', 
    director: '饺子', 
    year: '2023', 
    rating: 9.5, 
    genreId: 5, 
    tags: ['动画', '奇幻', '冒险'], 
    description: '哪吒再次踏上冒险之旅，面对更强大的敌人和更艰巨的挑战。', 
    link: 'https://movie.douban.com/subject/30110912/',
    isFavorite: true,
    isWatched: true
  },
  { 
    id: 3, 
    title: '疯狂动物城', 
    director: '拜伦·霍华德', 
    year: '2016', 
    rating: 9.2, 
    genreId: 5, 
    tags: ['动画', '冒险', '喜剧'], 
    description: '在一个所有哺乳动物和谐共存的动物城市中，兔子警官朱迪和狐狸尼克联手破案。', 
    link: 'https://movie.douban.com/subject/25662329/',
    isFavorite: false,
    isWatched: true
  },
  { 
    id: 4, 
    title: '速度与激情7', 
    director: '温子仁', 
    year: '2015', 
    rating: 8.3, 
    genreId: 3, 
    tags: ['动作', '犯罪', '惊悚'], 
    description: '多米尼克·托雷托和他的团队面对一个新的敌人，一个寻求复仇的男人。', 
    link: 'https://movie.douban.com/subject/23761370/',
    isFavorite: false,
    isWatched: false
  },
  { 
    id: 5, 
    title: '大话西游之大圣娶亲', 
    director: '刘镇伟', 
    year: '1995', 
    rating: 9.2, 
    genreId: 4, 
    tags: ['喜剧', '爱情', '奇幻'], 
    description: '至尊宝为救紫霞与牛魔王决战，最终还是与紫霞失之交臂。', 
    link: 'https://movie.douban.com/subject/1292213/',
    isFavorite: true,
    isWatched: true
  },
  { 
    id: 6, 
    title: '盗梦空间', 
    director: '克里斯托弗·诺兰', 
    year: '2010', 
    rating: 9.3, 
    genreId: 2, 
    tags: ['科幻', '悬疑', '冒险'], 
    description: '一个窃贼能够进入人们的梦中窃取秘密，被雇佣来植入一个想法到一个目标的潜意识中。', 
    link: 'https://movie.douban.com/subject/3541415/',
    isFavorite: true,
    isWatched: true
  }
]);

const selectedGenre = ref(0);

// 根据类型筛选电影
const filteredMovies = computed(() => {
  if (selectedGenre.value === 0) {
    return movies.value;
  }
  return movies.value.filter(movie => movie.genreId === selectedGenre.value);
});

// 收藏/取消收藏电影
const toggleFavorite = (movie: Movie) => {
  movie.isFavorite = !movie.isFavorite;
};

// 标记为已看/未看
const toggleWatched = (movie: Movie) => {
  movie.isWatched = !movie.isWatched;
};
</script> 