<template>
  <input
    class="input"
    type="text"
    :placeholder="placeholder"
    v-model="inputValue"
    v-on:input="valueUpdated" />
</template>

<script>
export default {
  name: 'TextInput',
  props: {
    value: String,
    valueChange: Function,
    placeholder: String
  },
  data: function () {
    return {
      inputValue: ''
    }
  },
  created () {
    this.inputValue = this.value
  },
  watch: {
    value () {
      this.inputValue = this.value
    }
  },
  methods: {
    valueUpdated: function () {
      this.valueChange(this.inputValue)
    }
  }
}
</script>

<style lang="scss" scoped>
  .input {
    width: calc(100% - 7px);
    font-size: 15px;
    border: 1px solid #000;
    border-radius: 6px;
    padding-left: 5px;
    height: 50px;
  }
</style>
