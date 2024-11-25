<script setup>
import { ref, computed } from 'vue';

const youtubeLink = ref('');
const numberOfTabs = ref(1);
const videoId = ref(null);
const videoTabs = ref([]);

const extractVideoId = (url) => {
  const regex = /(?:https?:\/\/)?(?:www\.)?(?:youtube\.com\/(?:[^/]+\/.+\/|(?:v|e(?:mbed)?)\/|.*[?&]v=)|youtu\.be\/)([^"&?/\s]{11})/;
  const match = url.match(regex);
  return match ? match[1] : null;
};

const playVideos = () => {
  const id = extractVideoId(youtubeLink.value);
  if (id) {
    videoId.value = id;
    videoTabs.value = Array.from({ length: Math.min(50, numberOfTabs.value) }, () => id); 
  } else {
    alert('Please enter a valid YouTube link!');
  }
};

const isValidTabCount = computed(() => numberOfTabs.value > 0 && numberOfTabs.value <= 50);
</script>

<template>
  <div class="min-h-screen flex flex-col items-center justify-center bg-gray-100 p-6">
    <!-- Input and Tab Count -->
    <div class="mb-6 w-full max-w-lg">
      <label for="youtube-link" class="block text-lg font-medium text-gray-700 mb-2">
        Enter YouTube Link
      </label>
      <div class="flex items-center mb-4">
        <input
          id="youtube-link"
          type="text"
          v-model="youtubeLink"
          placeholder="Paste YouTube link here"
          class="flex-1 p-3 border rounded-l-lg focus:outline-none focus:ring-2 focus:ring-blue-400"
        />
        <button
          @click="playVideos"
          class="p-3 bg-blue-600 text-white rounded-r-lg hover:bg-blue-700 transition"
        >
          Enter
        </button>
      </div>

      <label for="tab-count" class="block text-lg font-medium text-gray-700 mb-2">
        Number of Tabs (1-50)
      </label>
      <input
        id="tab-count"
        type="number"
        v-model.number="numberOfTabs"
        class="w-full p-3 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-400"
      />
    </div>

    <div v-if="videoTabs.length > 0" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 w-full max-w-7xl">
      <div
        v-for="(id, index) in videoTabs"
        :key="index"
        class="aspect-video rounded-lg border shadow-lg"
      >
      <iframe
  :src="`https://www.youtube.com/embed/${id}?autoplay=1&loop=1&playlist=${id}`"
  frameborder="0"
  allow="autoplay; encrypted-media"
  allowfullscreen
  class="w-full h-full rounded-lg"
></iframe>


      </div>
    </div>
  </div>
</template>
