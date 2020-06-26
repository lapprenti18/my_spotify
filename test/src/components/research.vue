<template>
  <div>
    <input v-model="search" v-on:keyup.enter="ytb_request(search)" />
    <button v-on:click="ytb_request(search)">
    SEARCH !
    </button>
    {{ search }}
    <ul>
      <li v-for="(item, index) in array" v-bind:key="index">
        <button v-on:click="ytb_play(item.id.videoId)">
          play !
        </button>
       {{ item.snippet.title }}
      </li>
    </ul>
  </div>
</template>

<script>
  import axios from "axios"
  import ytdl from "ytdl-core"
  // import HttpsProxyAgent from "https-proxy-agent"

  export default {
    data: function () {
      return {
        search:"",
        array:[]
      }
    },
    methods: {
      ytb_request: function(search) {
        this.array = [];
        this.search = '';

        axios
          .get(`https://www.googleapis.com/youtube/v3/search?part=snippet&type=video&maxResults=10&q=${search}&key=AIzaSyDk9rsLqzrKWqdER3AE3zdX9cDWUrkiG94`)
          .then((response) => { 
            console.log(response);
            for (let i = 0 ; i < response.data.items.length; i += 1)
              this.array.push(response.data.items[i]);
            console.log(this.array);
          })
      },
      ytb_play: (id) => {
        const stream = ytdl(`https://cors-anywhere.herokuapp.com/https://www.youtube.com/watch?v=${id}`, {
          // requestOptions: { agent },
        });
        stream.on('data', chunk => {
          console.log(chunk);
          console.log('downloaded', chunk.length);
        });
        console.log(id)
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