<template>
  <div class="selected-video">
    <!-- Video -->
    <div class="ratio ratio-16x9">
      <iframe
        width="100%"
        height="100%"
        :src="
          `https://www.youtube.com/embed/${selectedVideo.id.videoId}?autoplay=1&mute=1`
        "
        frameborder="0"
        allowfullscreen
      ></iframe>
    </div>
  </div>
</template>

<script>
export default {
  name: "SelectedVideo",
  props: {
    selectedVideo: {
      type: Object,
      required: true,
    },
  },
};
</script>
