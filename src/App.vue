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

  <div class="main">

    <div class="story-grid">
      <TransitionGroup>
      <div v-for="story in stories" :key="story.id" class="story-card">
        <div class="story-card-image">
          <img v-bind:src="story.hero_image.url" :style="'width:100%; height:100%; object-fit:cover;'">
        </div>
        <div class="story-card-text">
          <h2>{{ story.title }}</h2>
          <p v-html="story.dek"></p>
        </div>
      </div>
      </TransitionGroup>
    </div>
    
    <div class="navigation">
      <p v-if="!storiesLoaded" class="loading-text">
        Loading stories...
      </p>
      <button v-else v-on:click="addStories()" class="stories-btn">
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

.main {
  padding: 3rem;
}

.story-grid {
  display: grid;
  grid-template-columns: auto;
  gap: 2rem;
}

.story-card {
  border: 1px solid grey;
  border-radius: 1rem;
  background-color: white;
  display: grid;
  grid-template-rows: 30% auto;
  height: 20rem;
  overflow: scroll;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
}

.story-card-image {
  overflow: hidden;
  background-color: grey;
}

.story-card-text {
  padding: 2rem;
}

.navigation {
  margin-top: 3rem;
  display: flex;
  align-items: center;
  justify-content: center;
}

.loading-text {
  animation: updown 1s ease-in-out;
  animation-iteration-count: infinite;
}

.stories-btn {
  background-color: lightgrey;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  border: 1px solid grey;
  border-radius: 1rem;
  padding: 1rem;
  font-size: inherit;
  transition: 0.2s;
}

.stories-btn:hover {
  background-color: white;
}

@media only screen and (min-width: 48rem) {
  .story-grid {grid-template-columns: auto auto;}
  .story-card {height: 30rem;}
}

@media only screen and (min-width: 64rem) {
  .story-grid {grid-template-columns: auto auto auto;}
  .story-card {height: 40rem;}
}

@keyframes updown {
  0% {transform: translateY(0.5rem);}
  50% {transform: translateY(-0.5rem);}
  100% {transform: translateY(0.5rem);}
}

</style>
