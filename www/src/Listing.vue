<template>
  <Header></Header>
  <main class="container p-3">
    <h1>Home</h1>

    <div class="syllabi">
      <div>{{ msg }}</div>
      <ul>
        <li v-for="syllabus in syllabi">
          <div>
            <a :href="'/syllabi/' + syllabus.ID">{{ syllabus.title }}</a>
          </div>
          <div>{{ syllabus.description }}</div>
        </li>
      </ul>
    </div>
    <div class="cta">
      <button
        id="cta-upload"
        class="btn btn-primary mb-4 cc-btn"
        @click="start()"
      >
        Upload yours!
      </button>
    </div>
  </main>
  <Footer></Footer>
</template>

<script setup lang="ts">
  import { ref, onMounted, reactive } from 'vue'
  import Footer from './components/Footer.vue'
  import Header from './components/Header.vue'
  import { SyllabusType } from './js/types'

  const msg = ref('Loading Common Cartridges...')
  const HOST = import.meta.env.DEV ? 'http://localhost:3046' : ''
  const syllabi = new Array<SyllabusType>()

  let start = () => {
    window.location.href = '/index.html'
  }

  onMounted(() => {
    fetch(`${HOST}/syllabi/`, {
      method: 'GET',
    })
      .then((res) => {
        return res.json()
      })
      .then((data) => {
        Object.assign(syllabi, JSON.parse(data))
        console.log(syllabi)
        if (syllabi.length == 0) msg.value = 'No cartridges uploaded yet :('
        else msg.value = `There are ${syllabi.length} syllabi.`
      })
      .catch((err) => {
        console.error(err)
        msg.value = 'Network error :|'
      })
  })
</script>

<style lang="scss">
  @import './css/global-vars.scss';

  .syllabi {
    margin: 5em;
  }

  .cc-btn,
  .cc-btn:hover {
    border-radius: 25px;
    background-color: white;
    color: black;
    border-color: black;
  }

  .cc-btn:hover {
    text-decoration: underline;
  }
</style>
