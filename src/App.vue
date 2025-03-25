<script lang="ts">
export default {
  data() {
    return {
      shownPages: 0,
      stories: [] as {
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

    <div class="story-grid">

      <div v-for="story in stories" class="story-card">
        <div class="story-card-image">
          <img v-bind:src="story.hero_image.url" :style="'width:100%; height:100%; object-fit:cover;'">
        </div>
        <div class="story-card-text">
          <h3>{{ story.title }}</h3>
          <p v-html="story.dek"></p>
        </div>
      </div>

    </div>

    <div v-if="!storiesLoaded">
      Loading stories
    </div>
    <button v-else v-on:click="addStories()">More stories</button>
  
</template>

<style scoped>

.story-grid {
  display: grid;
  grid-template-columns: auto auto auto;
  grid-auto-rows: 1fr;
  gap: 2rem;
}

.story-card {
  border: 1px solid black;
  display: grid;
  grid-template-rows: 20rem auto;
}

.story-card-image {
  overflow: hidden;
  background-color: grey;
}

.story-card-text {
  padding: 1rem;
}

</style>
