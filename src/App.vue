<script lang="ts">
export default {
  data() {
    return {
      stories: [] as {title: string}[]
    }
  },
  methods: {
    async addStories(pageNumber:Number) {
      const url = "https://cryptodire.kontinentalist.com/api/v1/stories?page=" + pageNumber;
      try {
        const response = await fetch(url);
        if (!response.ok) {
          throw new Error(`Response status: ${response.status}`);
        }
        const json = await response.json();
        this.stories.push(...json.data);
      }
      catch (error) {
        console.log(error);
      }
    }
  },
  created() {
    this.addStories(1);
  }
}
</script>

<template>
  <div v-for="story in stories">
    {{ story.title }}
  </div>

  <button v-on:click="addStories(2)">more storie</button>
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
