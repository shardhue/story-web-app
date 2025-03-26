<script lang="ts">
export default {
  data() {
    return {
      shownPages: 0,
      stories: [] as {
        id: number,
        title: string,
        dek: string,
        hero_image: {url: string},
      }[],
      storiesLoaded: false
    }
  },
  methods: {
    async addStories() {
      this.storiesLoaded = false
      this.shownPages += 1;
      const url = "https://cryptodire.kontinentalist.com/api/v1/stories?page=" + this.shownPages;
      try {
        const response = await fetch(url);
        if (!response.ok) {
          throw new Error(`Response status: ${response.status}`);
        }
        const json = await response.json();
        this.stories.push(...json.data);
        this.storiesLoaded = true
      }
      catch (error) {
        console.log(error);
      }
    }
  },
  created() {
    this.addStories();
  }
}
</script>

<template>

  <div class="p-24">

    <div class="grid grid-cols-1 auto-rows-[30rem] md:auto-rows-[35rem] lg:auto-rows-[40rem] gap-8 md:grid-cols-2 lg:grid-cols-3">
      <TransitionGroup>
      <div v-for="story in stories" :key="story.id" class="border-1 border-gray-400 bg-white rounded-xl overflow-hidden
      shadow-md grid grid-rows-10">
        <div class="row-span-4 bg-gray-300">
          <img v-bind:src="story.hero_image.url" :style="'width:100%; height:100%; object-fit:cover;'">
        </div>
        <div class="row-span-6 p-8 overflow-scroll">
          <h2 class="text-xl font-bold">{{ story.title }}</h2>
          <hr class="my-2 border-gray-400">
          <p v-html="story.dek" class="text-sm"></p>
        </div>
      </div>
      </TransitionGroup>
    </div>
    
    <div class="mt-12 flex justify-center items-center h-16">
      <p v-if="!storiesLoaded" class="animate-bounce">
        Loading stories...
      </p>
      <button v-else v-on:click="addStories()" class="bg-gray-200 border-1 border-gray-400 shadow-md rounded-lg p-4
      transition duration-150 hover:bg-gray-300">
        More stories
      </button>
    </div>

  </div>
  
</template>

<style scoped>

.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}

</style>
