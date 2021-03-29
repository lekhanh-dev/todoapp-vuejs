<template>
  <Fragment>
    <h2>Search todo</h2>
    <input
      type="text"
      :value="textSearch"
      @input="changeTextSearch($event)"
      placeholder="Enter name to search"
    />
  </Fragment>
</template>

<script>
import { Fragment } from "vue-fragment";

export default {
  name: "control-search",
  props: {
    textSearch: String,
  },
  components: { Fragment },
  methods: {
    changeTextSearch(event) {
      let textSearch = event.target.value;
      this.$emit("changeTextSearch", textSearch);
    },
  },
};
</script>

<style></style>
