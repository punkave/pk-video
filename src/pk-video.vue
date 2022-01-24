<script>
import { defineComponent } from 'vue';

export default /*#__PURE__*/defineComponent({
  name: 'PkVideo', // vue component name
  data() {
    return {
      player: null,
      showOverlay: true,
      // setting inline styles for iframe here as a workaround
      iframeStyles: {
        width: `100%`,
        height: '100%',
        position: 'absolute',
        top: '0',
        left: '0',
      },
    };
  },
  props: {
    videoUrl: String,
    thumbnailUrl: String,
    buttonColor: String,
  },
  methods: {
    handlePlay() {
      this.showOverlay = false;
      this.player.playVideo();
    },
  },
  computed: {
    youtubeId() {
      if (this.videoUrl.includes('youtube')) {
        const split = this.videoUrl.split('v=');
        return split[split.length - 1];
      }
      return this.videoUrl;
    },
  },
  mounted() {
    youtube.load(YT => {
      this.player = new YT.Player('player', {
        videoId: this.youtubeId,
      });
    });
  }
});
</script>

<template>
  <div class="pk-video">
    <div class="pk-video__wrapper">
      <div id="player" :style="iframeStyles"></div>
      <transition name="overlay">
        <div
          class="pk-video__overlay"
          v-if="showOverlay"
          :style="{ backgroundImage: 'url(' + thumbnailUrl + ')' }"
        >
          <div @click="handlePlay">
            <slot></slot>
          </div>
        </div>
      </transition>
    </div>
  </div>
</template>

<style scoped>
.pk-video {
  position: relative;
  max-width: 100%;
  margin: 0 22px;
}

.pk-video__wrapper {
  height: 0;
  overflow: hidden;
  padding-top: 56.25%; /* 16x9 aspect ratio */
  position: relative;
}

.pk-video__overlay {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  display: flex;
  flex-direction: row;
  align-items: center;
  background-position: center;
  background-size: cover;
}

.overlay-enter,
.overlay-leave-to {
  opacity: 0;
}

.overlay-leave,
.overlay-enter-to {
  opacity: 1;
}

.overlay-enter-active,
.overlay-leave-active {
  transition: opacity 355ms;
}
</style>
