<template>
  <div id="research">
    <input class="search_bar" placeholder="search <3" v-model="search" v-on:keyup.enter="ytb_request(search)" />
    <button class="button" v-on:click="ytb_request(search)">
    SEARCH !
    </button>
    <div v-if="!display_player" >
      <ytb_tittle :search="search" :array="array" :video="video" @play="ytb_play" />
    </div>
    <div v-else-if="display_player">
      <player :video="video" :display_player="display_player" @bool="change_bool" />
    </div>
  </div>
</template>

<script>
  import axios from "axios"
  import player from './player.vue'
  import ytb_tittle from './ytb_tittle.vue'

  export default {
    components: {
      player,
      ytb_tittle,
    },

    data: function () {
      return {
        search:"",
        array:[],
        video:"",
        display_player: false,
      }
    },

    methods: {
      ytb_request: function(search) {
        this.array = [];
        this.search = '';
        this.display_player = false;

        axios
          .get(`https://www.googleapis.com/youtube/v3/search?part=snippet&type=video&maxResults=10&q=${search}&key=/* Put your key here*/`)
          .then((response) => { 
            for (let i = 0 ; i < response.data.items.length; i += 1)
              this.array.push(response.data.items[i]);
          })
      },
      ytb_play: function (item) {
        this.display_player = true;
        this.array = [];
        this.video = item;
      },
      change_bool: function (display_player) {
        this.display_player = false;
      }
    }
  }
</script>