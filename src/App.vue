<template>
  <div id="app" @mousemove="handleZoomed">
    <ul>
      <li v-for="(image, index) in images" :key="index">
        <img :src="image.url" :alt="image.title" @click="setCurrent(index)"/>
      </li>
    </ul>

    <section v-if="current !== null" class="modal" :class="{ zoomed: zoomed }" @click="clearCurrent">
      <img :src="images[current].url" :alt="images[current].title" @click.stop="toggleZoomed"/>
    </section>
  </div>
</template>

<script>
import 'normalize.css'

export default {
  name: 'app',
  data () {
    return {
      x: 0,
      y: 0,
      current: null,
      zoomed: false,
      images: [
        { title: "Bacon", url: "//picsum.photos/1200/800?image=34" },
        { title: "Juice", url: "//picsum.photos/1600/600?image=35" },
        { title: "Corn", url: "//picsum.photos/600?image=36" },
        { title: "Icecream", url: "//picsum.photos/800/1200?image=37" }
      ]
    }
  },
  methods: {
    setCurrent(item) {
      this.current = item
      this.zoomed = false
    },
    clearCurrent() {
      this.current = null
    },
    toggleZoomed() {
      this.zoomed = !this.zoomed
    },
    handleZoomed(e) {
      if(this.current) {
        this.x = e.clientX
        this.y = e.clientY
      }
    }
  }
}
</script>

<style>
  :root {
    --space: 1.25rem;
    --image-size: 20rem;
    --image-margin: calc(var(--space) * 4);
  }

  *,
  *::before,
  *::after {
    box-sizing: border-box;
  }

  img {
    max-width: 100%;
  }

  ul {
    list-style: none;
    padding: 0;
    margin: 0;
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(var(--image-size), 1fr));
  }

  .modal {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    display: grid;
    align-items: center;
    justify-content: center;
    background: hsla(0, 0%, 10%, 0.9);
    overflow-y: auto;
  }

  .modal img {
    max-height: 100%;
  }

  .zoomed img {
    max-width: unset;
    max-height: unset;
    margin: var(--image-margin);
  }
</style>
