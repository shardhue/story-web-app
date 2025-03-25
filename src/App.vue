<script lang="ts">
export default {
  data() {
    return {
      shownPages: 0,
      stories: [] as {title: string}[],
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

  <div v-for="story in stories">
    {{ story.title }}
  </div>

  <div v-if="!storiesLoaded">
    Loading stories
  </div>

  <button v-on:click="addStories()">more stories</button>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
