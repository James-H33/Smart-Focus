<template>
  <div id="app">
    <div class="url-list-wrapper">
      <h2>Smart Focus</h2>

      <UrlList
        class="url-list-component"
        :urls="urlList"
        :updated="UpdatedUrls" />

      <TextInput
        :value="url"
        :placeholder="'Add a Website'"
        :valueChange="UpdateUrl" />

      <div class="message error" v-if="errorMessage">
        {{errorMessage}}
      </div>

      <Button :onClick="AddUrl" :label="'Add'" />
    </div>

    <div class="options">
      <SingleSelector
        class="selector"
        :selected="options.timeLimit"
        :label="'Time Limit'"
        :values="timeLimits"
        :valueChanged="TimeLimitSelectionChange" />

      <SingleSelector
        class="selector"
        :selected="options.numberOfQuestions"
        :label="'Number Of Questions'"
        :values="numberOfQuestions"
        :valueChanged="NumberOfQuestionsSelectionChange" />

      <SingleSelector
        class="selector"
        :selected="options.deck"
        :label="'Pick from your Decks'"
        :values="decks"
        :valueChanged="DeckSelectionChange" />
    </div>
  </div>
</template>

<script>
import TextInput from './components/TextInput.vue'
import Button from './components/Button.vue'
import UrlList from './components/UrlList.vue'
import SingleSelector from './components/SingleSelector.vue'

export default {
  name: 'App',
  data: function () {
    return {
      url: '',
      errorMessage: '',
      urlList: [],
      timeLimits: [
        { label: '5 minutes', value: 5 },
        { label: '10 minutes', value: 10 },
        { label: '15 minutes', value: 15 }
      ],
      numberOfQuestions: [
        { label: '3 questions', value: 3 },
        { label: '5 questions', value: 5 },
        { label: '10 questions', value: 10 }
      ],
      decks: [
        { label: 'Ultimate French Language Pack', value: 3 },
        { label: 'All things Dogs', value: 5 },
        { label: 'Step 2 Questions 2020', value: 10 },
        { label: '4000 Essential English Words (all books) [en-en] M4R4M', value: 15 }
      ],
      options: {
        deck: 3,
        timeLimit: 5,
        numberOfQuestions: 3
      }
    }
  },
  created () {
    const urls = JSON.parse(localStorage.getItem('sf-urls')) ?? []
    const options = JSON.parse(localStorage.getItem('sf-options'))

    this.urlList = [...urls]
    this.options = options ?? this.BuildNewOptions()
  },
  components: {
    TextInput,
    Button,
    UrlList,
    SingleSelector
  },
  methods: {
    UpdateUrl (url) {
      this.url = url
    },
    AddUrl () {
      this.errorMessage = ''

      if (this.url) {
        if (!this.IsUrlFormat()) {
          this.errorMessage = 'Url does not match format. https://example.com'
          return
        }

        if (this.IsDuplicateUrlEntry()) {
          this.errorMessage = 'This url is already in the collection!'
          return
        }

        this.urlList.push({ url: this.url.toLowerCase() })
        this.AddListToStorage()
        this.url = ''
      }
    },
    AddListToStorage () {
      localStorage.setItem('sf-urls', JSON.stringify(this.urlList))
    },
    IsUrlFormat () {
      return /(https?:\/\/)\w+\.\w{2,}/.test(this.url.toLowerCase())
    },
    IsDuplicateUrlEntry () {
      return Boolean(this.urlList.find(item => item.url === this.url))
    },
    UpdatedUrls (urls) {
      this.urlList = urls
      this.AddListToStorage()
    },
    TimeLimitSelectionChange (newTime) {
      this.options.timeLimit = newTime
      this.UpdateOptionsInStorage()
    },
    NumberOfQuestionsSelectionChange (n) {
      this.options.numberOfQuestions = n
      this.UpdateOptionsInStorage()
    },
    DeckSelectionChange (n) {
      this.options.deck = n
      this.UpdateOptionsInStorage()
    },
    UpdateOptionsInStorage () {
      localStorage.setItem('sf-options', JSON.stringify(this.options))
    },
    BuildNewOptions () {
      return {
        timeLimit: 5,
        numberOfQuestions: 3,
        deck: 3
      }
    }
  }
}
</script>

<style lang="scss">
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    display: grid;
    grid-template-columns: 1fr 1fr;
    column-gap: 2em;
    max-width: 900px;
    margin: 0 auto;
    padding-top: 60px;
  }

  h2 {
    margin-bottom: 2em;
  }

  .url-list-wrapper {
    padding: 20px;

    input {
      margin-bottom: 10px;
    }

    button {
      width: 100%;
    }
  }

  .message {
    margin-bottom: 10px;
    text-align: left;
  }

  .url-list-component {
    margin-bottom: 1em;
  }

  .options {
    padding: 20px;

    .selector {
      margin-bottom: 30px;
    }
  }
</style>
