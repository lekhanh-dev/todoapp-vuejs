<template>
  <section class="control">
    <control-search
      :textSearch="textSearch"
      @changeTextSearch="changeTextSearch"
    />
  </section>
</template>

<script>
import ControlSearch from "./ControlSearch.vue";

export default {
  name: "comp-control",
  props: {
    textSearch: String,
  },
  components: {
    ControlSearch,
  },
  methods: {
    changeTextSearch(text) {
      this.$emit("changeTextSearch", text);
    },
  },
};
</script>

<style></style>
