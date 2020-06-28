<template>
  <ul>
    <li v-for="(item, index) in array" v-bind:key="index">
      <button class="button" v-on:click="ytb_play(item)">
        play !
      </button>
      <div class="ytb_title">
        {{ item.snippet.title }}
      </div>
    </li>
  </ul>
</template>

<script>
  export default {
    props: {
      search: String,
      array: Array,
      method: { type: Function },
    },
    methods: {
      ytb_play: function (item) {
        this.$emit('play', item);
      }
    }
  }
</script>