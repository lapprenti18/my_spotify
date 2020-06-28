<template>
  <div id="research">
    <input class="button" v-model="search" v-on:keyup.enter="ytb_request(search)" />
    <button class="button" v-on:click="ytb_request(search)">
    SEARCH !
    </button>
    <div v-if="!display_player" >
      <ul>
        <li v-for="(item, index) in array" v-bind:key="index" >
          <button class="button" v-on:click="ytb_play(item)">
            play !
          </button>
         {{ item.snippet.title }}
        </li>
      </ul>
    </div>
    <div v-else-if="display_player" >
      <iframe id="ytplayer" type="text/html" width="640" height="360"
        :src="'https://www.youtube.com/embed/' + video.id.videoId + '?autoplay=1&origin=http://localhost:8080/'"
        frameborder="0">
      </iframe>
      <button  class="button" v-on:click="display_player = false">
        stop !
      </button>
    </div>
  </div>
</template>

<script>
  import axios from "axios"
  // import ytdl from "ytdl-core"
  // import HttpsProxyAgent from "https-proxy-agent"

  export default {
    el: "#research",
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

        axios                                                                                                 /*  put you r google key here */
          .get(`https://www.googleapis.com/youtube/v3/search?part=snippet&type=video&maxResults=10&q=${search}&key= Put your key here`)
          .then((response) => { 
            console.log(response);
            for (let i = 0 ; i < response.data.items.length; i += 1)
              this.array.push(response.data.items[i]);
            console.log(this.array);
          })
      },
      ytb_play: function (video) {
        this.display_player = true;
        this.array = [];
        this.video = video;
        console.log(`https://www.youtube.com/embed/${video.id.videoId}?autoplay=1&origin=http://localhost:8080/`);
        // const stream = ytdl(`https://cors-anywhere.herokuapp.com/https://www.youtube.com/watch?v=${id}`, {
        //   // requestOptions: { agent },
        // });
        // stream.on('data', chunk => {
        //   console.log(chunk);
        //   console.log('downloaded', chunk.length);
        // });
        // console.log(id)
        // ytdl.getInfo(id, {}, async(err, info) => {
        //   if (err) console.log(err);
        //   let audioFormats = ytdl.filterFormats(info.formats, 'audioonly');
        //   const sound = new Audio.Sound();
        //   console.log(sound);
        //   console.log(audioFormats);
        // })

      },
    }
  }
</script>