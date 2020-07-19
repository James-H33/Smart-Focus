<template>
  <button @click="clicked" :class="buttonClasses">{{label}}</button>
</template>

<script>
export default {
  name: 'Button',
  props: {
    label: String,
    classes: String,
    onClick: Function
  },
  computed: {
    buttonClasses () {
      let classes = 'button'

      if (this.classes) {
        classes = `${classes} ${this.classes}`
      }

      return classes
    }
  },
  methods: {
    clicked () {
      if (this.onClick) {
        this.onClick()
      }
    }
  }

}
</script>

<style lang="scss">
  .button {
    color: #fff;
    font-size: 15px;
    height: 50px;
    border-radius: 6px;
    padding: 10px 20px;
    border: none;
    background: #40b882;
  }

</style>
