<template>
  <div id="app">
    <div id="nav" class="d-flex p-3 gap-sm-5">
      <a href="#" class="text-primary text-decoration-none me-5">
        <img alt="Vue logo" src="./assets/logo.png" width="60" />
        <span
          style="color: rgb(53,73,94); font-size: 32px; vertical-align: middle; margin-left: -18px; font-weight: 900; letter-spacing: .03rem"
          >Tube</span
        >
      </a>
      <div class="py-2 flex-grow-1 ms-sm-5">
        <SearchBar @search="fetchVideos" :searchTerm="searchTerm" />
      </div>
    </div>

    <!-- Errors -->
    <div
      v-if="errors"
      ref="error"
      class="alert alert-danger mx-3"
      role="alert"
    ></div>

    <div class="row m-0">
      <!-- SelectedVideo -->
      <template v-if="selectedVideo">
        <div :class="videoLayout">
          <SelectedVideo :selectedVideo="selectedVideo" />
          <SelectedVideoDetails
            v-if="layout != 'theater'"
            :selectedVideo="selectedVideo"
            :layout="layout"
            @changedLayout="changeLayout"
          />
        </div>
      </template>

      <!-- VideoList - after search -->
      <template v-if="videos.length && !selectedVideo">
        <div>
          <VideoList
            :videos="videos"
            :layout="layout"
            @videoSelected="selectVideo"
          />
        </div>
      </template>

      <!-- VideoList in Sidebar -->
      <template v-if="selectedVideo && layout != 'theater'">
        <div class="col-lg-4">
          <VideoList
            :videos="videos"
            :layout="layout"
            @videoSelected="selectVideo"
          />
        </div>
      </template>

      <!-- VideoList in Sidebar w/ comments -->
      <template v-if="selectedVideo && layout == 'theater'">
        <div class="row m-0">
          <div class="col-lg-8 ps-0 pe-0 pe-lg-3">
            <SelectedVideoDetails
              v-if="layout == 'theater'"
              :selectedVideo="selectedVideo"
              :layout="layout"
              @changedLayout="changeLayout"
            />
          </div>
          <div class="col-lg-4 ps-0 pe-0 pe-lg-3">
            <VideoList
              :videos="videos"
              :layout="layout"
              @videoSelected="selectVideo"
            />
          </div>
        </div>
      </template>
    </div>

    <!-- <div id="video-list-bottom"></div> -->
  </div>
</template>

<script>
/* eslint-disable */
import axios from "axios";
// import scrollMonitor from "scrollmonitor";

import SearchBar from "@/components/SearchBar.vue";
import VideoList from "@/components/VideoList.vue";
import SelectedVideo from "@/components/SelectedVideo.vue";
import SelectedVideoDetails from "@/components/SelectedVideoDetails.vue";

// let watcher;

export default {
  name: "App",
  components: {
    SearchBar,
    VideoList,
    SelectedVideo,
    SelectedVideoDetails,
  },
  data() {
    return {
      searchTerm: "trucks 2022",
      searchResults: [],
      videos: [],
      selectedVideo: null,
      layout: "",
      errors: false,
      maxSearchResults: 45, // 9 * 3 - load 9 each time on scroll
    };
  },
  methods: {
    fetchVideos(q) {
      if (q == this.searchTerm) return;
      this.searchTerm = q;
      this.selectedVideo = null;
      this.errors = false;
      this.layout = "";
      axios
        .get("https://www.googleapis.com/youtube/v3/search", {
          params: {
            key: process.env.VUE_APP_API_KEY,
            type: "video",
            part: "snippet",
            maxResults: this.maxSearchResults,
            q,
          },
        })
        .then(({ data }) => {
          this.searchResults = data.items;
          this.videos = data.items.slice(0, 9);
        })
        .catch((error) => {
          this.errors = true;
          setTimeout(() => {
            this.$refs.error.innerHTML = error.response.data.error.message;
          });
        });
    },
    selectVideo(videoId) {
      this.selectedVideo = this.videos.filter(
        (video) => video.etag == videoId
      )[0];
      this.layout = "sidebar";
      // let sensor = document.getElementById("video-list-bottom");
      // watcher = scrollMonitor.create(sensor);
      // watcher.enterViewport(this.appendResults);
    },
    changeLayout() {
      this.layout = this.layout == "theater" ? "sidebar" : "theater";
    },
    // appendResults() {
    //   console.log("append results");
    // },
  },
  computed: {
    videoLayout() {
      return this.layout == "theater" ? "col-sm-12" : "col-lg-8";
    },
    videoListLayout() {
      return this.layout == "theater" ? "col-sm-12" : "col-lg-4";
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
