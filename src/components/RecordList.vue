<template>
  <div class="top-albums">
    <div class="top-albums-list">
      <div
        v-for="(album, i) in albums"
        :key="album.index"
        :class="{ active: i == selectedAlbum }"
        class="album"
      >
        <div
          class="album-bg"
          :style="{ backgroundImage: 'url(' + album.cover + ')' }"
        />
        <img class="album-fg" :src="album.cover" :alt="album.title" />
        <div class="album-title">
          <span class="album-title-number">{{ i + 1 }}</span> {{ album.title }}
        </div>
      </div>
    </div>
    <div class="top-albums-nav">
      <a
        class="navigate prev"
        @click="prevAlbum"
        :disabled="selectedAlbum === 0"
      >
        <svg id="icon-prev" viewBox="0 0 100 50" width="100%" height="100%">
          <polygon
            points="5.4,25 18.7,38.2 22.6,34.2 16.2,27.8 94.6,27.8 94.6,22.2 16.2,22.2 22.6,15.8 18.7,11.8"
          ></polygon>
        </svg>
      </a>
      <a
        class="navigate next"
        @click="nextAlbum"
        :disabled="selectedAlbum === albums.length - 1"
      >
        <svg id="icon-next" viewBox="0 0 100 50" width="100%" height="100%">
          <polygon
            points="81.3,11.8 77.4,15.8 83.8,22.2 5.4,22.2 5.4,27.8 83.8,27.8 77.4,34.2 81.3,38.2 94.6,25 "
          ></polygon>
        </svg>
      </a>
    </div>
  </div>
</template>

<script>
import albumsJson from "../assets/albums.json";

export default {
  name: "RecordList",
  data() {
    return {
      loaded: false,
      selectedAlbum: 0,
      albums: albumsJson
    };
  },
  methods: {
    nextAlbum() {
      this.selectedAlbum++;
    },
    prevAlbum() {
      this.selectedAlbum--;
    }
  }
};
</script>

<style lang="scss" scoped>
$darkblue: #24263f;
$blue: #2a4468;
$lightblue: #117cd9;
$white: #e7e9f5;
$red: #ed3224;
$purp: #24263f;

.top-albums {
  height: 100vh;
  position: relative;
  width: 100vw;
  max-width: 1600px;
  margin: 0 auto;
}

.top-albums-list {
  pointer-events: none;

  .album {
    height: 100%;
    left: 0;
    margin: 0;
    opacity: 0;
    padding: 6em 0;
    pointer-events: none;
    position: absolute;
    top: 0;
    width: 100%;

    &.active {
      opacity: 1;
    }

    .album-bg {
      background-position: top right;
      background-repeat: no-repeat;
      background-size: 65%;
      filter: blur(5px);
      height: 100vh;
      opacity: 0.1;
      position: absolute;
      right: 0;
      top: 0;
      width: 100%;
      z-index: 1;
    }

    .album-fg {
      position: relative;
      z-index: 20;
    }

    .album-title {
      color: $white;
      display: inline-block;
      font-size: 6em;
      left: 0;
      line-height: 0.8;
      margin: 0;
      padding: 1em 0 0;
      pointer-events: none;
      position: absolute;
      top: 70%;
      z-index: 20;

      .album-title-number {
        color: $red;
      }
    }
  }
}

.top-albums-nav {
  bottom: 2em;
  position: absolute;
  right: 2em;
  z-index: 100;

  .navigate {
    cursor: pointer;
    display: block;
    height: 50px;
    transition: transform 150ms ease-in-out;
    width: 100px;
    z-index: 20;

    &.prev {
      margin-left: -3rem;

      &:active {
        transform: translateX(-0.5rem);
      }
    }

    &.next {
      &:active {
        transform: translateX(0.5rem);
      }
    }

    svg {
      filter: drop-shadow(1px 1px 2px rgba(0,0,0,.25));
      pointer-events: none;

      polygon {
        fill: $red;
      }
    }

    &:hover,
    &:active,
    &:focus {
      svg polygon {
        fill: $white;
      }
    }

    &[disabled] {
      filter: drop-shadow(1px 1px 2px rgba(0,0,0,.65));
      pointer-events: none;

      svg polygon {
        fill: $purp;
      }
    }
  }
}
</style>
