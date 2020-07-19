<template>
  <div id="app">
    <div class="url-list-wrapper">
      <h2>Smart Focus</h2>

      <UrlList :urls="urlList" />

      <TextInput
        class="url-list-input"
        :value="url"
        :placeholder="'Add a Website'"
        :valueChange="UpdateUrl" />

      <div v-if="errorMessage">{{errorMessage}}</div>

      <Button :onClick="AddUrl" :label="'Add'" />
    </div>
  </div>
</template>

<script>
import TextInput from './components/TextInput.vue'
import Button from './components/Button.vue'
import UrlList from './components/UrlList.vue'

export default {
  name: 'App',
  data: function () {
    return {
      url: '',
      errorMessage: '',
      urlList: [{ url: 'https://hello.com' }]
    }
  },
  created () {
    const urls = JSON.parse(localStorage.getItem('sf-urls') ?? '[]')

    this.urlList = [...urls]
  },
  components: {
    TextInput,
    Button,
    UrlList
  },
  methods: {
    UpdateUrl (url) {
      this.url = url
    },
    AddUrl () {
      this.errorMessage = ''

      if (this.url && this.IsUrlFormat()) {
        this.urlList.push({ url: this.url.toLowerCase() })
        this.AddListToStorage()
        this.url = ''
      } else {
        this.errorMessage = 'Url does not match format. https://example.com'
      }
    },
    AddListToStorage () {
      localStorage.setItem('sf-urls', JSON.stringify(this.urlList))
    },
    IsUrlFormat () {
      return /(https?:\/\/)\w+\.\w{2,}/.test(this.url.toLowerCase())
    },
    RemoveUrl (url) {
      this.urlList = this.urlList.filter(x => x !== url)
      this.AddListToStorage()
    }
  }
}
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
    display: grid;
    grid-template-columns: 1fr 1fr;
    max-width: 900px;
    margin: 0 auto;
    padding-top: 60px;
  }

  h2 {
    margin-bottom: 3em;
  }

  .url-list-wrapper {
    display: grid;
    width: 100%;
    padding: 20px;
  }

  .url-list-input {
    margin-bottom: 10px;
  }

  .button {
    min-width: 170px
  }

  .url-list {
    display: grid;
    margin-bottom: 1em;
  }
</style>
