<template>
  <button v-on:click="input(item)">
    <div>{{ item }}</div>
  </button>
</template>

<script>
export default {
  name: "numberIn",
  props: {
    item: Number,
  },
  data() {
    return {};
  },
  methods: {
    input: function (inputItem) {
      this.$emit("outputNumber", inputItem);
    },
  },
};
</script>

<style>
button {
  padding: 0;
  padding-bottom: 4px;
  background-color: rgb(229, 229, 231);
}
button div {
  font-size: 35px;
  font-weight: 600;
  border-radius: 8px;
  background-color: rgb(255, 255, 255);
}
</style>
