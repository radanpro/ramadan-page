<template>
  <section class="section-video">
    <h2 class="video-title">أجواء رمضانية 🎥</h2>
    <div
      class="video-container"
      @mouseenter="showControls = true"
      @mouseleave="showControls = false"
    >
      <video
        ref="videoPlayer"
        :src="videoSrc"
        autoplay
        :muted="isMuted"
        loop
        playsinline
        class="video-player"
      ></video>
      <div class="video-overlay"></div>

      <!-- Custom Controls -->
      <div class="video-controls" :class="{ 'controls-visible': showControls }">
        <button
          class="control-btn"
          @click="toggleMute"
          :title="isMuted ? 'تشغيل الصوت' : 'كتم الصوت'"
        >
          <span v-if="isMuted">🔇</span>
          <span v-else>🔊</span>
        </button>

        <div class="speed-control">
          <button
            v-for="speed in speeds"
            :key="speed"
            class="speed-btn"
            :class="{ active: currentSpeed === speed }"
            @click="setSpeed(speed)"
          >
            {{ speed }}x
          </button>
        </div>
      </div>
    </div>
  </section>
</template>

<script lang="ts">
import videoFile from "@/assets/videos/video1.mp4";

export default {
  name: "BackgroundVideo",
  data() {
    return {
      videoSrc: videoFile,
      isMuted: true,
      currentSpeed: 1,
      speeds: [0.5, 1, 1.5, 2],
      showControls: false,
    };
  },
  methods: {
    toggleMute() {
      this.isMuted = !this.isMuted;
    },
    setSpeed(speed: number) {
      this.currentSpeed = speed;
      const video = this.$refs.videoPlayer as HTMLVideoElement;
      if (video) {
        video.playbackRate = speed;
      }
    },
  },
};
</script>

<style scoped>
.section-video {
  width: 100%;
  margin: 20px 0 40px;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}

.video-title {
  margin-bottom: 25px;
  width: 100%;
}

.video-container {
  position: relative;
  width: 100%;
  max-width: 900px;
  aspect-ratio: 16 / 9;
  border-radius: 32px;
  overflow: hidden;
  box-shadow:
    0 30px 60px -12px rgba(0, 0, 0, 0.5),
    0 18px 36px -18px rgba(0, 0, 0, 0.5);
  background: var(--card-bg);
  border: 1px solid var(--glass-border);
  transition: transform 0.5s cubic-bezier(0.2, 0.8, 0.2, 1);
}

.video-container:hover {
  transform: translateY(-5px) scale(1.01);
}

.video-player {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.video-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(
    to bottom,
    transparent 40%,
    rgba(0, 0, 0, 0.4) 100%
  );
  pointer-events: none;
}

/* Custom Controls Styles */
.video-controls {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%) translateY(20px);
  background: rgba(15, 23, 42, 0.8);
  backdrop-filter: blur(12px);
  padding: 10px 20px;
  border-radius: 20px;
  display: flex;
  align-items: center;
  gap: 20px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  opacity: 0;
  transition: all 0.4s cubic-bezier(0.2, 0.8, 0.2, 1);
  z-index: 10;
}

.video-controls.controls-visible {
  opacity: 1;
  transform: translateX(-50%) translateY(0);
}

.control-btn {
  background: none;
  border: none;
  font-size: 1.5rem;
  cursor: pointer;
  padding: 5px;
  border-radius: 50%;
  transition: background 0.3s;
  display: flex;
  align-items: center;
  justify-content: center;
}

.control-btn:hover {
  background: rgba(255, 255, 255, 0.1);
}

.speed-control {
  display: flex;
  gap: 8px;
  border-right: 1px solid rgba(255, 255, 255, 0.1);
  padding-right: 15px;
  order: -1;
}

.speed-btn {
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  color: #fff;
  padding: 4px 10px;
  border-radius: 12px;
  font-size: 0.85rem;
  cursor: pointer;
  transition: all 0.3s;
  font-family: "Cairo", sans-serif;
}

.speed-btn:hover {
  background: rgba(255, 255, 255, 0.15);
  border-color: var(--accent-color);
}

.speed-btn.active {
  background: var(--accent-color);
  color: #000;
  border-color: var(--accent-color);
  font-weight: 700;
}

@media (max-width: 768px) {
  .video-container {
    border-radius: 20px;
  }

  .video-title {
    font-size: 1.5rem;
    margin-bottom: 20px;
  }

  .video-controls {
    padding: 8px 15px;
    gap: 15px;
    bottom: 15px;
  }

  .speed-btn {
    padding: 3px 8px;
    font-size: 0.75rem;
  }
}
</style>
