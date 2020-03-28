<template>
  <div id="app">
    <title>Simple YouTube Player</title>
    <link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Sen">
    <center>
      <main>
          <div class="search">
            <input type="text" class="search-box" placeholder="Enter a YouTube Song URL" v-model="url" @keypress.enter="fetchYoutube" />
          </div><br><br>
          <div class="wrapper">
            <h1>Simple YouTube Player</h1>
            <div class="player" v-if="typeof song.title != 'undefined'">
              <img :src="song.thumbnail" alt="thumb" class="thumb"><br>
              <h2>{{ song.title }}</h2><br>
              <h3>{{ song.author }}</h3><br>
              <audio class="play" preload controls autoplay loop>
                <source :src="song.url">
              </audio>
            </div>
          </div>
      </main>
    </center>
  </div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      url: '',
      song: {}
    }
  },
  methods: {
    fetchYoutube() {
      const ytdl = require("ytdl-core");
      ytdl.getInfo(this.url, (err, res) => {
        if(err) return console.log('Error');
        let song = res.formats.sort((a, b) => a.bitrate - b.bitrate)[0];
        if(!song) return console.log('No Song under this Bitrate');
        this.setResults({
          title: res.title,
          thumbnail: `https://i.ytimg.com/vi/${res.video_id}/maxresdefault.jpg`,
          url: song.url,
          author: res.author.name
        });
      });
    },
    setResults(res) {
      this.song = res;
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Sen';
  color: #FFF;
  background-image: url('./assets/bg.jpg');
  background-size: cover;
  background-position: bottom;
}

#app {
  transition: 0.5s;
  height: 100vh;
  background: linear-gradient(to bottom, rgba(0, 0, 0, 0), rgba(0, 0, 0, 0.7));
}

main {
  padding: 30px 30px;
}

.search-box {
  display: block;
  width: 100%;
  padding: 13px 15px;
  color: #313131;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  font-size: 20px;
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 15px 0 15px 0;
  transition: 0.75s;
  box-shadow: 0 0 10px rgba(255, 255, 255, 0.4);
}

.search-box:focus {
  background-color: #FFF;
  border-radius: 0 15px 0 15px;
  box-shadow: 0 0 10px rgba(255, 255, 255, 0.90);
}

.thumb {
  border-radius: 10px;
  width: 50%;
  margin-bottom: 15px;
}

.player {
  margin-top: 20px;
  padding: 20px;
  background-color: rgba(255, 255, 255, 0.4);
  border-radius: 20px;
}

h1 {
  text-shadow: 0 0 4px rgb(0, 0, 0);
}

.play {
  background-color: #f1f3f4;
  border-radius: 14px;
  width: 70%;
}
</style>
