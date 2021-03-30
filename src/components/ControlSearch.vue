<template>
  <div>
    <h2>Search todo</h2>
    <input
      type="text"
      :value="textSearch"
      @input="changeTextSearch($event)"
      placeholder="Enter name to search"
    />
  </div>
</template>

<script>
export default {
  name: "control-search",
  props: {
    textSearch: String,
  },
  methods: {
    changeTextSearch(event) {
      let textSearch = event.target.value;
      this.$emit("changeTextSearch", textSearch);
    },
  },
};
</script>

<style></style>
