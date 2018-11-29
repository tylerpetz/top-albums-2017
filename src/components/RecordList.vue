<template>
  <div class="top-albums">
    <section class="top-albums__list">
      <article class="album">
        <transition name="cover" mode="out-in">
          <figure class="album__cover" :key="selectedAlbum">
            <img
              class="album__cover--img"
              :src="album.cover"
              :alt="album.artist"
            />
          </figure>
        </transition>
        <header class="album__details">
          <transition name="artist" mode="out-in">
            <h1 class="album__artist" :key="selectedAlbum">
              {{ album.artist }}
            </h1>
          </transition>
          <transition name="album" mode="out-in">
            <h2 class="album__title" :key="selectedAlbum">{{ album.title }}</h2>
          </transition>
        </header>
        <transition name="bg" mode="out-in">
          <div
            class="album__bg"
            :style="{ backgroundImage: 'url(' + album.cover + ')' }"
            :key="selectedAlbum"
          />
        </transition>
      </article>
    </section>
    <section class="top-albums__nav">
      <a class="nav-item prev" @click="prevAlbum" :disabled="firstAlbum">
        <svg id="icon-prev" viewBox="0 0 100 50" width="100%" height="100%">
          <polygon
            points="5.4,25 18.7,38.2 22.6,34.2 16.2,27.8 94.6,27.8 94.6,22.2 16.2,22.2 22.6,15.8 18.7,11.8"
          />
        </svg>
      </a>
      <a class="nav-item next" @click="nextAlbum" :disabled="lastAlbum">
        <svg id="icon-next" viewBox="0 0 100 50" width="100%" height="100%">
          <polygon
            points="81.3,11.8 77.4,15.8 83.8,22.2 5.4,22.2 5.4,27.8 83.8,27.8 77.4,34.2 81.3,38.2 94.6,25"
          />
        </svg>
      </a>
    </section>
  </div>
</template>

<script>
import _ from "lodash";
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
  computed: {
    firstAlbum() {
      return this.selectedAlbum === 0;
    },
    lastAlbum() {
      return this.selectedAlbum === this.albums.length - 1;
    },
    album() {
      return {
        artist: this.albums[this.selectedAlbum].artist,
        title: this.albums[this.selectedAlbum].title,
        cover: this.albums[this.selectedAlbum].cover
      };
    }
  },
  methods: {
    nextAlbum: _.debounce(function() {
      this.selectedAlbum++;
    }, 300),
    prevAlbum: _.debounce(function() {
      this.selectedAlbum--;
    }, 300)
  },
  created: function() {
    let self = this;

    window.addEventListener("keyup", function(e) {
      if (e.keyCode == 37) {
        self.prevAlbum();
      } else if (e.keyCode == 39) {
        self.nextAlbum();
      }
    });
  }
};
</script>

<style lang="scss" scoped>
$blue: #247ba0;
$green: #70c1b3;
$lightgreen: #b2dbbf;
$yellow: #f3ffbd;
$red: #ff1654;

.top-albums {
  &__list {
    align-items: center;
    display: flex;
    height: 100vh;
    justify-content: center;
    width: 100vw;
  }

  &__nav {
    bottom: 2em;
    position: absolute;
    right: 2em;
    z-index: 100;
  }
}

.nav-item {
  cursor: pointer;
  display: block;
  height: 50px;
  transition: transform 150ms ease;
  width: 100px;
  z-index: 20;

  &.prev {
    margin-left: -3rem;

    &:active:not[disabled] {
      transform: translateX(-0.25rem);
    }
  }

  &.next {
    &:active:not([disabled]) {
      transform: translateX(0.25rem);
    }
  }

  svg {
    filter: drop-shadow(1px 1px 2px rgba(0, 0, 0, 0.25));
    pointer-events: none;

    polygon {
      fill: #ed393a;
      transition: fill 250ms ease;
    }
  }

  &[disabled] {
    pointer-events: none;

    svg polygon {
      fill: #333333;
    }
  }
}

.album {
  align-items: center;
  display: flex;
  height: 300px;
  margin: 0;
  position: relative;
  width: 1024px;

  & * {
    perspective: 1000px;
    transform-style: preserve-3d;
  }

  &__bg {
    background-position: center right;
    background-repeat: no-repeat;
    background-size: 100%;
    filter: blur(20px);
    height: 100%;
    opacity: 0.1;
    position: fixed;
    right: 0;
    top: 0;
    width: 100%;
    z-index: 1;
  }

  &__cover {
    margin: 0;
    perspective: 1000px;
    transform-style: preserve-3d;
    user-select: none;
    width: 300px;

    img {
      width: 300px;
      max-width: 300px;
    }
  }

  &__details {
    display: flex;
    flex-direction: column;
    flex-grow: 1;
    height: 100%;
    padding-left: 1.5em;
  }

  &__artist {
    color: #ececec;
    display: inline-block;
    font-size: 2.5em;
    font-style: italic;
    font-weight: 400;
    line-height: 1.6;
    margin: 0;
    user-select: none;
    z-index: 20;
  }

  &__title {
    color: #4d9de0;
    font-size: 4em;
    font-weight: 400;
    line-height: 1;
    margin: 0;
    user-select: none;
    z-index: 20;
  }
}

.artist-enter-active {
  transition: all 0.35s ease;
}

.artist-leave-active {
  transition: all 0.25s cubic-bezier(1, 0.5, 0.8, 1);
}

.artist-enter,
.artist-leave-to {
  opacity: 0;
  transform: translate3d(20px, 0, 100px);
}

.album-enter-active {
  transition: all 0.35s ease;
}

.album-leave-active {
  transition: all 0.25s cubic-bezier(1, 0.5, 0.8, 1);
}

.album-enter,
.album-leave-to {
  opacity: 0;
  transform: translate3d(20px, 20px, 100px);
}

.bg-enter-active,
.bg-leave-active {
  transition: opacity 0.15s;
}

.bg-enter,
.bg-leave-to {
  opacity: 0;
}

.cover-enter-active {
  transition: all 0.3s ease;
}

.cover-leave-active {
  transition: all 0.25s cubic-bezier(1, 0.5, 0.8, 1);
}

.cover-enter,
.cover-leave-to {
  opacity: 0;
  transform: perspective(1400px) translate3d(0px, 0px, 350px);
}
</style>
