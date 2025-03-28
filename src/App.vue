<template>

  <SiteHeader/>

  <div class="p-24 pt-16">

    <div class="grid grid-cols-1 auto-rows-[30rem] md:auto-rows-[35rem] lg:auto-rows-[40rem] gap-12 md:grid-cols-2 lg:grid-cols-3">
      <TransitionGroup>
      <StoryCard v-for="story in stories" :key="story.id" :title="story.title" :dek="story.dek" :heroImageUrl="story.hero_image.url"/>
      </TransitionGroup>
    </div>
    
    <div class="mt-12 flex justify-center items-center h-16">
      <p v-if="!storiesLoaded" class="animate-bounce">
        Loading stories...
      </p>
      <button v-else v-on:click="addStories()" class="bg-teal-600 shadow-md rounded-lg p-4 transition duration-150 
      hover:bg-gray-400 text-white font-bold">
        More stories
      </button>
    </div>

  </div>
  
</template>

<script lang="ts">
import SiteHeader from './components/SiteHeader.vue';
import StoryCard from './components/StoryCard.vue';

export default {
  components: {
    SiteHeader,
    StoryCard
  },
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
    //adds page 1 of the stories on site load
    this.addStories();
  }
}
</script>

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
