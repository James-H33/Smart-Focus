<template>
  <div class="single-selector">
    <h4>{{label}}</h4>
    <div class="single-selector-list">
      <label v-for="item in values" :key="item.value">
        <input
          type="radio"
          :name="groupName"
          :value="item.value"
          v-model="_selected"
          :checked="_selected === item.value">
        <div class="single-selector-label">{{item.label}}</div>
      </label>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SingleSelector',
  props: {
    selected: String,
    label: String,
    values: Array,
    valueChanged: Function
  },
  data: function () {
    return {
      currentSelection: null,
      groupName: `group-${(Math.random() * 1000).toFixed(0)}`
    }
  },
  computed: {
    _selected: {
      get: function () {
        return this.currentSelection
      },
      set: function (v) {
        this.currentSelection = v
        this.valueChanged(v)
      }
    }
  },
  created () {
    this._selected = this.selected || this.values[0].value
  }
}
</script>

<style lang="scss" scoped>
  .single-selector {
    display: grid;
    justify-items: left;

    h4 {
      margin-bottom: 5px;
    }
  }

  .single-selector-list {
    label {
      display: grid;
      grid-template-columns: 20px 1fr;
      align-items: center;
      margin-bottom: 5px;
      cursor: pointer;
    }
  }

  .single-selector-label {
    text-align: left;
  }
</style>
