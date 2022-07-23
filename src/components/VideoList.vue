<template>
  <div class="video-list">
    <h4 class="mt-2">{{ videoListHeading }}</h4>
    <ul
      v-if="videos.length > 0"
      class="list-group list-group-flush p-0"
      style="list-style-type: none"
    >
      <VideoListItem
        v-for="video in videos"
        :key="video.etag"
        :video="video"
        :layout="layout"
        @videoSelected="$emit('videoSelected', $event)"
      />
    </ul>

    <!-- <div id="video-list-bottom"></div> -->
  </div>
</template>

<script>
// import scrollMonitor from "scrollmonitor";

import VideoListItem from "@/components/VideoListItem";

// let watcher;

export default {
  name: "VideoList",
  props: {
    videos: {
      type: Array,
      required: true,
    },
    layout: {
      type: String,
      default: "",
    },
  },
  components: {
    VideoListItem,
  },
  methods: {
    // appendResults() {
    //   console.log("append results");
    // },
  },
  computed: {
    videoListHeading() {
      return this.layout == "sidebar" || this.layout == "theater"
        ? "Other Videos"
        : "Results";
    },
  },
  // updated() {
  //   let sensor = document.getElementById("video-list-bottom");
  //   watcher = scrollMonitor.create(sensor);
  //   watcher.enterViewport(this.appendResults);
  // },
  // beforeUpdate() {
  //   if (watcher) {
  //     watcher.destroy();
  //     watcher = null;
  //   }
  // },
};
</script>
