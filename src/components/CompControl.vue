<template>
  <section class="control">
    <h2>Search todo</h2>
    <input
      type="text"
      :value="textSearch"
      @input="$emit('changeTextSearch', $event.target.value)"
      placeholder="Enter name to search"
    />
  </section>
</template>

<script>
export default {
  name: "comp-control",
  props: {
    textSearch: String,
  },
};
</script>
