<template>
  <div>
    <iframe id="ytplayer" type="text/html" width="640" height="360"
      :src="'https://www.youtube.com/embed/' + video.id.videoId + '?autoplay=1&origin=http://localhost:8080/'"
      frameborder="0">
    </iframe>
    <button  class="button" v-on:click="change_bool(display_player)">
      stop !
    </button>
  </div>
</template>

<script>
  export default {
    props: {
      display_player: Boolean,
      video: Object,
      method: { type: Function },
    },
    methods: {
      change_bool: function (display_player) {
        this.$emit('bool', display_player);
      }
    }
  }
</script>