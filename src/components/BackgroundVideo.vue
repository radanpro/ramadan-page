<template>
  <section class="section-video">
    <h2 class="video-title">أجواء رمضانية 🎥</h2>
    <div class="video-container">
      <video
        :src="videoSrc"
        autoplay
        muted
        loop
        playsinline
        class="video-player"
      ></video>
      <div class="video-overlay"></div>
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
    };
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

@media (max-width: 768px) {
  .video-container {
    border-radius: 20px;
  }

  .video-title {
    font-size: 1.5rem;
    margin-bottom: 20px;
  }
}
</style>
