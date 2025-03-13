<template>
  <div>
    <div class="mb-8">
      <h1 class="text-3xl font-bold text-gray-800 dark:text-white mb-4">音乐分享</h1>
      <p class="text-gray-600 dark:text-gray-400">这里分享我喜欢的音乐，希望你也能喜欢。</p>
    </div>

    <div class="mb-8">
      <div class="flex flex-wrap gap-4">
        <button 
          v-for="genre in musicGenres" 
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

    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      <div 
        v-for="music in filteredMusic" 
        :key="music.id" 
        class="bg-white dark:bg-gray-800 rounded-lg shadow-md overflow-hidden"
      >
        <div class="h-48 bg-gray-200 dark:bg-gray-700 flex items-center justify-center relative group">
          <p class="text-gray-500 dark:text-gray-400">{{ music.title }} - 封面</p>
          <button 
            @click="playMusic(music)"
            class="absolute inset-0 flex items-center justify-center bg-black bg-opacity-0 group-hover:bg-opacity-30 transition-all duration-300 opacity-0 group-hover:opacity-100"
          >
            <Icon 
              :name="currentlyPlaying?.id === music.id ? 'ph:pause-circle-fill' : 'ph:play-circle-fill'" 
              class="w-16 h-16 text-white"
            />
          </button>
        </div>
        <div class="p-5">
          <h3 class="font-bold text-gray-800 dark:text-white text-lg">{{ music.title }}</h3>
          <p class="text-gray-600 dark:text-gray-400 mt-1">{{ music.artist }}</p>
          
          <div class="mt-4 flex justify-between items-center">
            <div>
              <span class="text-xs text-gray-500 dark:text-gray-400 block">{{ music.album }}</span>
              <span class="text-xs text-gray-500 dark:text-gray-400 block mt-1">{{ music.year }}</span>
            </div>
            <div class="flex space-x-2">
              <span class="inline-block bg-gray-100 dark:bg-gray-700 text-gray-600 dark:text-gray-300 text-xs px-2 py-1 rounded">
                {{ getMusicGenreName(music.genreId) }}
              </span>
            </div>
          </div>
          
          <p class="text-gray-600 dark:text-gray-400 text-sm mt-4">{{ music.description }}</p>
          
          <div v-if="currentlyPlaying?.id === music.id" class="mt-4">
            <div class="w-full bg-gray-200 dark:bg-gray-700 rounded-full h-1.5 mb-2">
              <div class="bg-indigo-600 h-1.5 rounded-full" :style="{ width: `${playbackProgress}%` }"></div>
            </div>
            <div class="flex justify-between text-xs text-gray-500 dark:text-gray-400">
              <span>{{ formatTime(currentTime) }}</span>
              <span>{{ formatTime(music.duration) }}</span>
            </div>
          </div>
          
          <div class="mt-4 flex justify-between items-center">
            <a :href="music.link" target="_blank" class="text-indigo-600 dark:text-indigo-400 text-sm hover:underline">
              在音乐平台上听
            </a>
            <button @click="toggleFavorite(music)" class="text-gray-500 dark:text-gray-400 hover:text-red-500 dark:hover:text-red-400">
              <Icon :name="music.isFavorite ? 'ph:heart-fill' : 'ph:heart'" class="w-6 h-6" :class="{ 'text-red-500': music.isFavorite }" />
            </button>
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

interface MusicGenre {
  id: number;
  name: string;
}

interface Music {
  id: number;
  title: string;
  artist: string;
  album: string;
  year: string;
  duration: number; // 秒
  genreId: number;
  description: string;
  link: string;
  isFavorite: boolean;
}

// 音乐类型
const musicGenres = ref<MusicGenre[]>([
  { id: 0, name: '全部' },
  { id: 1, name: '流行' },
  { id: 2, name: '摇滚' },
  { id: 3, name: '民谣' },
  { id: 4, name: '电子' },
  { id: 5, name: '古典' }
]);

// 音乐数据
const musicList = ref<Music[]>([
  { 
    id: 1, 
    title: '晴天', 
    artist: '周杰伦', 
    album: '叶惠美', 
    year: '2003', 
    duration: 269, 
    genreId: 1, 
    description: '这首歌曲是周杰伦的经典作品之一，旋律优美，歌词感人。', 
    link: 'https://music.163.com/#/song?id=186016',
    isFavorite: true
  },
  { 
    id: 2, 
    title: 'Yellow', 
    artist: 'Coldplay', 
    album: 'Parachutes', 
    year: '2000', 
    duration: 266, 
    genreId: 2, 
    description: 'Coldplay的成名曲，温暖而感人的摇滚歌曲。', 
    link: 'https://music.163.com/#/song?id=3986241',
    isFavorite: false
  },
  { 
    id: 3, 
    title: '南山南', 
    artist: '马頔', 
    album: '南山南', 
    year: '2013', 
    duration: 252, 
    genreId: 3, 
    description: '一首充满诗意的民谣，描绘了对远方的向往和对过去的怀念。', 
    link: 'https://music.163.com/#/song?id=28793140',
    isFavorite: true
  },
  { 
    id: 4, 
    title: 'Strobe', 
    artist: 'Deadmau5', 
    album: 'For Lack of a Better Name', 
    year: '2009', 
    duration: 618, 
    genreId: 4, 
    description: '电子音乐的经典之作，层次分明，节奏感强。', 
    link: 'https://music.163.com/#/song?id=4010243',
    isFavorite: false
  },
  { 
    id: 5, 
    title: '月光奏鸣曲', 
    artist: '贝多芬', 
    album: '钢琴奏鸣曲全集', 
    year: '1801', 
    duration: 360, 
    genreId: 5, 
    description: '贝多芬最著名的钢琴奏鸣曲之一，优美而富有感染力。', 
    link: 'https://music.163.com/#/song?id=5264843',
    isFavorite: true
  },
  { 
    id: 6, 
    title: '七里香', 
    artist: '周杰伦', 
    album: '七里香', 
    year: '2004', 
    duration: 296, 
    genreId: 1, 
    description: '周杰伦的经典情歌，旋律优美，歌词浪漫。', 
    link: 'https://music.163.com/#/song?id=186001',
    isFavorite: false
  }
]);

const selectedGenre = ref(0);
const currentlyPlaying = ref<Music | null>(null);
const currentTime = ref(0);
const playbackProgress = ref(0);
const playbackInterval = ref<number | null>(null);

// 根据类型筛选音乐
const filteredMusic = computed(() => {
  if (selectedGenre.value === 0) {
    return musicList.value;
  }
  return musicList.value.filter(music => music.genreId === selectedGenre.value);
});

// 获取音乐类型名称
const getMusicGenreName = (genreId: number): string => {
  const genre = musicGenres.value.find(g => g.id === genreId);
  return genre ? genre.name : '';
};

// 播放音乐
const playMusic = (music: Music) => {
  if (currentlyPlaying.value?.id === music.id) {
    // 暂停当前播放的音乐
    currentlyPlaying.value = null;
    if (playbackInterval.value) {
      clearInterval(playbackInterval.value);
      playbackInterval.value = null;
    }
    currentTime.value = 0;
    playbackProgress.value = 0;
  } else {
    // 播放新的音乐
    currentlyPlaying.value = music;
    currentTime.value = 0;
    playbackProgress.value = 0;
    
    // 模拟播放进度
    if (playbackInterval.value) {
      clearInterval(playbackInterval.value);
    }
    
    playbackInterval.value = window.setInterval(() => {
      if (currentTime.value < music.duration) {
        currentTime.value += 1;
        playbackProgress.value = (currentTime.value / music.duration) * 100;
      } else {
        // 播放结束
        currentlyPlaying.value = null;
        if (playbackInterval.value) {
          clearInterval(playbackInterval.value);
          playbackInterval.value = null;
        }
        currentTime.value = 0;
        playbackProgress.value = 0;
      }
    }, 1000);
  }
};

// 格式化时间
const formatTime = (seconds: number): string => {
  const minutes = Math.floor(seconds / 60);
  const remainingSeconds = Math.floor(seconds % 60);
  return `${minutes}:${remainingSeconds < 10 ? '0' : ''}${remainingSeconds}`;
};

// 收藏/取消收藏音乐
const toggleFavorite = (music: Music) => {
  music.isFavorite = !music.isFavorite;
};
</script> 