<template>

  <div class="border-1 border-gray-300 bg-white rounded-xl overflow-hidden shadow-md grid grid-rows-10">
    <div class="row-span-4 bg-gray-300">
      <img v-bind:src="heroImageUrl" :style="'width:100%; height:100%; object-fit:cover;'">
    </div>
    <div ref="textbox" class="row-span-6 p-8 overflow-scroll" v-on:scroll="checkScrollLength" v-bind:class="{
      'unscrolled': scrollable && scrollTop && !scrollBottom,
      'scrolling': scrollable && !scrollTop && !scrollBottom,
      'scrolled': scrollable && !scrollTop && scrollBottom}">
      <h2 class="text-xl font-bold">{{ title }}</h2>
      <hr class="my-2 border-gray-400">
      <p v-html="dek" class="text-sm"></p>
    </div>
  </div>
  
</template>

<script lang="ts">
export default {
  props: {
      title: String,
      dek: String,
      heroImageUrl: String
  },
  data() {
    return {
      scrollable: false,
      scrollBottom: false,
      scrollTop: true
    }
  },
  methods: {
    checkScrollLength() {
      const cardText = this.$refs['textbox'] as HTMLDivElement

      //checks if the card text can be scrolled
      if (cardText.scrollHeight == cardText.clientHeight) {
        this.scrollable = false;
      }
      else if (cardText.scrollHeight != cardText.clientHeight) {
        this.scrollable = true;
      }

      //checks if card text has been scrolled to the top
      if (cardText.scrollTop == 0) {
        this.scrollTop = true;
      }
      else {
        this.scrollTop = false;
      }
      
      //checks if card text has been scrolled to the bottom
      if (Math.abs(cardText.scrollHeight - cardText.clientHeight - cardText.scrollTop) <= 1) {
        this.scrollBottom = true;
      }
      else {
        this.scrollBottom = false;
      }
    }
  },
  mounted() {
    this.checkScrollLength();
  }
}
</script>

<style scoped>

.unscrolled {
  box-shadow: inset 0rem -3rem 2rem -2rem rgba(0, 0, 0, 0.15);
  transition: 0.15s;
}

.scrolling {
  box-shadow: inset 0rem -3rem 2rem -2rem rgba(0, 0, 0, 0.15), inset 0rem 3rem 2rem -2rem rgba(0, 0, 0, 0.15);
  transition: 0.15s;
}

.scrolled {
  box-shadow: inset 0rem 3rem 2rem -2rem rgba(0, 0, 0, 0.15);
  transition: 0.15s;
}

</style>

  