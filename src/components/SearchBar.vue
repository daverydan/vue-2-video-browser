<template>
  <div class="search-bar">
    <div class="d-flex justify-content-between mb-2">
      <input
        type="text"
        @keydown.enter="search"
        class="form-control me-md-2"
        style="font-size: 16px"
        placeholder="Search"
      />
      <button type="button" class="btn btn-primary w-25 d-none d-md-block">
        Search
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "SearchBar",
  props: {
    searchTerm: {
      type: String,
      required: true,
    },
  },
  methods: {
    search(event) {
      if (event.target.value.length) {
        this.$emit("search", event.target.value);
      }
    },
  },
};
</script>
