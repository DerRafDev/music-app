
<!-- This is for importing the style.css -->
<style>
  @import './assets/styles/style.css';
</style>

<template>
  <div id="app">
    <header>
      <h1>Music Playing</h1>
    </header>
    <main>
      <section class="player"> 
        <!-- Using the data to show the song title -->
        <h2 class="song-title">{{ current.title }} - <span>{{ current.artist }}</span></h2>
        <div class="contro">
          <button class="prev" @click="prev">Prev</button>
          <button class="play" v-if="!isPlaying" @click="play">Play</button>
          <!-- v-if is for showing the button Play if is not playing or v-else to
                show Pause if is playing the music -->
          <button class="pause" v-else @click="pause">Pause</button>
          <button class="next" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <!-- This is for showing the playlist -->
        <h3>Playlist</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)"
          :class="( song.src == current.src) ? 'song playing' : 'song'">

          {{ song.title }} - {{ song.artist }}

        </button>
      </section>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data: function () {
    return {
      current: { },
      index: 0,
      isPlaying: false,
      songs: [
        {
          //as this is just an visual application, we will put the name and artist by hand
          title: 'Beethoven Overdosed',
          artist: 'Janne Nummela',
          src: require('./assets/Janne_Nummela_-_01_-_Beethoven_Overdosed.mp3')
        },
        {
          title: 'No Peddlers 45',
          artist: 'Vivaldi',
          src: require('./assets/No_Peddlers_-_45_-_Vivaldi.mp3')
        }
      ],
      player: new Audio ()
    }
  },
  methods: {
    play (song) {
      //this is for playing the music
      if (typeof song.src != "undefined") {
          this.current = song;

          this.player.src = this.current.src;
       }

      this.player.play();

      this.player.addEventListener('ended', function () {
        this.index++;
        if (this.index > this.songs.length - 1) {
          this.index = 0;
          // if we click in the next song but we are on the last one, it will get back to the first song
        }

        this.current = this.songs[this.index];
        this.play(this.current);
      }.bind(this));
      
      this.isPlaying = true;
    },

    //this will make this.isPlaying become false to stop the music
    pause () {
      this.player.pause();
      this.isPlaying = false;
    },

    //this is for the button next song
    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
        // if we click in the next song but we are on the last one, it will get back to the first song
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    },

    //this is for the button prev song
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
        // if we click in the prev song but we are on the first one, it will get to the end of the playlist
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created() {
    //this is for showing the current value in the array songs
    this.current = this.songs[this.index]
    this.player.src = this.current.src;
  }
}
</script>
