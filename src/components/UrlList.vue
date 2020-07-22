<template>
  <div class="url-list">
    <div class="url-list-item" v-for="(item, i)  in values" :key="i">
      <div class="label">{{item.url}}</div>
      <div class="url-list-item-close" @click="removeUrl(i)">
        &times;
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'UrlList',
  props: {
    urls: Array,
    updated: Function
  },
  computed: {
    values () {
      return this.urls || []
    }
  },
  methods: {
    removeUrl (index) {
      const newList = this.urls.filter((url, i) => i !== index)
      this.updated(newList)
    }
  }
}
</script>

<style lang="scss" scoped>
  .url-list {
    display: grid;
  }

  .url-list-item {
    display: grid;
    grid-template-columns: 1fr 25px;
    justify-content: space-between;
    align-items: center;

    &:hover {
      background: #f5f5f5;
    }

    .label {
      text-align: left;
    }
  }

  .url-list-item-close {
    color: red;
    font-weight: bold;
    font-size: 24px;
    cursor: pointer;
  }
</style>
