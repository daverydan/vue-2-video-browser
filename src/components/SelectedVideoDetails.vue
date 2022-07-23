<template>
  <div class="selected-video-details">
    <div class="d-flex justify-content-between mt-3 mb-3">
      <!-- Subscribe -->
      <button class="btn btn-danger btn-sm">Subscribe</button>
      <!-- Theater Layout -->
      <button
        class="btn btn-primary btn-sm"
        @click="$emit('changedLayout')"
        :title="layoutButtonTitle"
      >
        <span class="d-flex align-items-center">
          <span
            class="theater-icon border border-2 border-white d-inline-block"
          ></span>
          <span
            v-if="layout == 'theater'"
            class="sidebar-icon border border-2 border-white d-inline-block ms-1"
          ></span>
        </span>
      </button>
    </div>

    <p class="mb-3">
      <strong>{{ selectedVideo.snippet.title }}</strong>
    </p>

    <p>{{ selectedVideo.snippet.description }}</p>

    <p class="mb-5"><em class="text-secondary">Comments are turned off.</em></p>
  </div>
</template>

<script>
export default {
  props: {
    selectedVideo: {
      type: Object,
    },
    layout: {
      type: String,
    },
  },
  computed: {
    layoutButtonTitle() {
      return this.layout == "theater" ? "Sidebar" : "Theater";
    },
  },
};
</script>

<style scoped>
.selected-video-details .theater-icon {
  height: 15px;
  width: 20px;
}
.selected-video-details .sidebar-icon {
  height: 15px;
  width: 8px;
}
</style>
