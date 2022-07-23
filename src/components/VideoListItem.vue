<template>
  <li class="video-list-item list-group-item p-0">
    <div
      @click.prevent="$emit('videoSelected', video.etag)"
      class="video-item-link cursor-pointer py-3"
    >
      <div class="row">
        <!-- Video -->
        <div class="col-sm-5 pe-sm-0">
          <div class="ratio ratio-16x9">
            <div
              style="background-repeat: no-repeat; background-size: cover; background-position: center;"
              :style="{
                backgroundImage: `url(${video.snippet.thumbnails.high.url})`,
              }"
            ></div>
          </div>
        </div>
        <!-- Title & Description -->
        <div class="col-sm-7">
          <strong class="video-item__title">{{ video.snippet.title }}</strong>
          <br />
          <span>{{ video.snippet.channelTitle }}</span>
          <span v-if="layout == ''">{{ video.snippet.description }}</span>
        </div>
      </div>
    </div>
  </li>
</template>

<script>
export default {
  name: "VideoItem",
  props: {
    video: {
      type: Object,
    },
    layout: {
      type: String,
      default: "",
    },
  },
};
</script>

<style scoped>
.cursor-pointer {
  cursor: pointer;
}
.video-item__title {
  color: #000;
  transition: all 200ms ease-in-out;
}
.video-item-link {
  background: #fff;
  transition: all 200ms ease-in-out;
}
.video-item-link:hover {
  background: #f4f4f4;
}
.video-item-link:hover .video-item__title {
  color: red;
}
.video-item__channel {
  font-size: 13px;
  margin-top: 3px;
}
</style>
