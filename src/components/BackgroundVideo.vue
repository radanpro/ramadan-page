<template>
  <section class="section-video">
    <div class="video-header">
      <div class="video-title-badge">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="18"
          height="18"
          viewBox="0 0 24 24"
          fill="currentColor"
          class="video-title-icon"
        >
          <path
            d="M12 2L2 7l10 5 10-5-10-5zM2 17l10 5 10-5M2 12l10 5 10-5"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          />
        </svg>
      </div>
      <h2 class="video-title">أجواء رمضانية</h2>
    </div>

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

      <!-- Gradient Overlay -->
      <div class="video-overlay"></div>

      <!-- Custom Controls -->
      <div class="video-controls" :class="{ 'controls-visible': showControls }">
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

        <div class="controls-divider"></div>

        <button
          class="control-btn"
          @click="toggleMute"
          :title="isMuted ? 'تشغيل الصوت' : 'كتم الصوت'"
        >
          <!-- Muted icon -->
          <svg
            v-if="isMuted"
            xmlns="http://www.w3.org/2000/svg"
            width="18"
            height="18"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <polygon points="11 5 6 9 2 9 2 15 6 15 11 19 11 5"></polygon>
            <line x1="23" y1="9" x2="17" y2="15"></line>
            <line x1="17" y1="9" x2="23" y2="15"></line>
          </svg>
          <!-- Unmuted icon -->
          <svg
            v-else
            xmlns="http://www.w3.org/2000/svg"
            width="18"
            height="18"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <polygon points="11 5 6 9 2 9 2 15 6 15 11 19 11 5"></polygon>
            <path
              d="M19.07 4.93a10 10 0 0 1 0 14.14M15.54 8.46a5 5 0 0 1 0 7.07"
            ></path>
          </svg>
        </button>
      </div>

      <!-- Ramadan watermark -->
      <div class="video-watermark">🌙</div>
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
/* Section */
.section-video {
  width: 100%;
  margin-bottom: 8px;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}

/* Header */
.video-header {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 12px;
  margin-bottom: 22px;
  width: 100%;
}

.video-title-badge {
  width: 38px;
  height: 38px;
  border-radius: 12px;
  background: var(--accent-soft, rgba(245, 200, 66, 0.12));
  border: 1px solid rgba(245, 200, 66, 0.2);
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--accent-color, #f5c842);
  flex-shrink: 0;
}

.video-title-icon {
  display: block;
}

.video-title {
  margin-bottom: 0 !important;
  font-size: 1.6rem !important;
}

/* Video Container */
.video-container {
  position: relative;
  width: 100%;
  max-width: 900px;
  aspect-ratio: 16 / 9;
  border-radius: 24px;
  overflow: hidden;
  border: 1px solid var(--glass-border, rgba(255, 255, 255, 0.07));
  box-shadow:
    0 32px 64px -20px rgba(0, 0, 0, 0.7),
    0 0 0 1px rgba(255, 255, 255, 0.03) inset;
  background: #05070d;
  transition:
    transform 0.6s cubic-bezier(0.2, 0.8, 0.2, 1),
    box-shadow 0.6s cubic-bezier(0.2, 0.8, 0.2, 1);
  cursor: pointer;
}

.video-container:hover {
  transform: translateY(-4px);
  box-shadow:
    0 44px 80px -20px rgba(0, 0, 0, 0.8),
    0 0 0 1px rgba(245, 200, 66, 0.08) inset;
}

/* Video Player */
.video-player {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

/* Gradient Overlay */
.video-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(
    to bottom,
    rgba(3, 5, 9, 0.05) 0%,
    transparent 30%,
    transparent 60%,
    rgba(3, 5, 9, 0.5) 100%
  );
  pointer-events: none;
}

/* Watermark */
.video-watermark {
  position: absolute;
  top: 16px;
  right: 16px;
  font-size: 1.4rem;
  opacity: 0.4;
  pointer-events: none;
  filter: drop-shadow(0 2px 8px rgba(0, 0, 0, 0.5));
}

/* Controls */
.video-controls {
  position: absolute;
  bottom: 18px;
  left: 50%;
  transform: translateX(-50%) translateY(12px);
  background: rgba(10, 15, 30, 0.85);
  backdrop-filter: blur(16px) saturate(150%);
  -webkit-backdrop-filter: blur(16px) saturate(150%);
  padding: 8px 16px;
  border-radius: 50px;
  display: flex;
  align-items: center;
  gap: 12px;
  border: 1px solid rgba(255, 255, 255, 0.08);
  opacity: 0;
  transition: all 0.4s cubic-bezier(0.2, 0.8, 0.2, 1);
  z-index: 10;
  white-space: nowrap;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.4);
}

.video-controls.controls-visible {
  opacity: 1;
  transform: translateX(-50%) translateY(0);
}

.controls-divider {
  width: 1px;
  height: 20px;
  background: rgba(255, 255, 255, 0.1);
  flex-shrink: 0;
}

/* Mute button */
.control-btn {
  background: rgba(255, 255, 255, 0.06);
  border: 1px solid rgba(255, 255, 255, 0.08);
  color: var(--text-primary, #f1f5f9);
  width: 34px;
  height: 34px;
  border-radius: 50%;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
  flex-shrink: 0;
}

.control-btn:hover {
  background: rgba(255, 255, 255, 0.12);
  border-color: var(--accent-color, #f5c842);
  color: var(--accent-color, #f5c842);
  transform: scale(1.08);
}

/* Speed buttons */
.speed-control {
  display: flex;
  gap: 4px;
}

.speed-btn {
  background: transparent;
  border: 1px solid rgba(255, 255, 255, 0.08);
  color: var(--text-secondary, #94a3b8);
  padding: 4px 10px;
  border-radius: 20px;
  font-size: 0.78rem;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.25s ease;
  font-family: "Cairo", sans-serif;
  letter-spacing: 0;
}

.speed-btn:hover {
  background: rgba(255, 255, 255, 0.08);
  border-color: rgba(245, 200, 66, 0.3);
  color: var(--text-primary, #f1f5f9);
}

.speed-btn.active {
  background: var(--accent-color, #f5c842);
  border-color: var(--accent-color, #f5c842);
  color: #03050a;
  font-weight: 800;
  box-shadow: 0 0 12px rgba(245, 200, 66, 0.35);
}

/* Mobile */
@media (max-width: 640px) {
  .video-container {
    border-radius: 18px;
  }

  .video-title {
    font-size: 1.35rem !important;
  }

  .video-controls {
    padding: 6px 12px;
    gap: 8px;
    bottom: 12px;
  }

  .speed-btn {
    padding: 3px 8px;
    font-size: 0.72rem;
  }

  .control-btn {
    width: 30px;
    height: 30px;
  }
}
</style>
