<template>
  <div class="moon-container">
    <div class="moon-glow"></div>
    <div class="moon-orbit-ring"></div>
    <div class="moon-wrapper" v-html="svgMoon"></div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "MoonDisplay",
  props: {
    phase: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      svgMoon: "",
    };
  },
  watch: {
    phase: {
      immediate: true,
      handler(newVal) {
        this.svgMoon = this.renderMoonSVG(newVal);
      },
    },
  },
  methods: {
    renderMoonSVG(phase: number) {
      const r = 50;
      const cosPhi = Math.cos(phase * 2 * Math.PI);
      const mag = Math.abs(cosPhi) * r;
      const isWaxing = phase <= 0.5;
      let path = "";

      if (isWaxing) {
        const sweepOuter = 1,
          sweepInner = phase < 0.25 ? 0 : 1;
        path = `M 50 0 A 50 50 0 0 ${sweepOuter} 50 100 A ${mag} 50 0 0 ${sweepInner} 50 0`;
      } else {
        const sweepOuter = 0,
          sweepInner = phase < 0.75 ? 1 : 0;
        path = `M 50 0 A 50 50 0 0 ${sweepOuter} 50 100 A ${mag} 50 0 0 ${sweepInner} 50 0`;
      }

      return `
        <svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg" class="moon-svg">
          <defs>
            <radialGradient id="moonSurface" cx="38%" cy="35%" r="65%">
              <stop offset="0%" style="stop-color:#FAFAF5;stop-opacity:1"/>
              <stop offset="50%" style="stop-color:#E8E8DA;stop-opacity:1"/>
              <stop offset="100%" style="stop-color:#CCC9B8;stop-opacity:1"/>
            </radialGradient>
            <radialGradient id="moonGlow" cx="50%" cy="50%" r="50%">
              <stop offset="0%" style="stop-color:rgba(255,255,220,0.05);stop-opacity:1"/>
              <stop offset="100%" style="stop-color:rgba(0,0,0,0);stop-opacity:1"/>
            </radialGradient>
            <filter id="moonInnerShadow" x="-20%" y="-20%" width="140%" height="140%">
              <feComponentTransfer in="SourceAlpha">
                <feFuncA type="table" tableValues="1 0" />
              </feComponentTransfer>
              <feGaussianBlur stdDeviation="4"/>
              <feOffset dx="-3" dy="-3" result="offsetblur"/>
              <feFlood flood-color="#000020" flood-opacity="0.5" result="color"/>
              <feComposite in2="offsetblur" operator="in"/>
              <feComposite in2="SourceAlpha" operator="in" />
              <feMerge>
                <feMergeNode in="SourceGraphic" />
                <feMergeNode />
              </feMerge>
            </filter>
            <filter id="softGlow" x="-50%" y="-50%" width="200%" height="200%">
              <feGaussianBlur stdDeviation="2" result="blur"/>
              <feMerge>
                <feMergeNode in="blur"/>
                <feMergeNode in="SourceGraphic"/>
              </feMerge>
            </filter>
            <mask id="moonMask"><circle cx="50" cy="50" r="49" fill="white"/></mask>
          </defs>
          <!-- Dark background circle -->
          <circle cx="50" cy="50" r="49" fill="#04060c"/>
          <!-- Moon surface -->
          <path d="${path}" fill="url(#moonSurface)" filter="url(#moonInnerShadow)"/>
          <!-- Craters -->
          <g mask="url(#moonMask)" opacity="0.09" style="transform-origin:50% 50%;transform:rotate(12deg)">
            <circle cx="34" cy="33" r="7.5" fill="#000"/>
            <circle cx="64" cy="44" r="10" fill="#000"/>
            <circle cx="45" cy="68" r="5.5" fill="#000"/>
            <circle cx="27" cy="61" r="4.5" fill="#000"/>
            <circle cx="72" cy="27" r="6.5" fill="#000"/>
            <circle cx="50" cy="19" r="3.5" fill="#000"/>
            <circle cx="14" cy="45" r="4" fill="#000"/>
            <circle cx="84" cy="57" r="3.5" fill="#000"/>
            <circle cx="58" cy="80" r="4" fill="#000"/>
          </g>
        </svg>
      `;
    },
    animateMoon() {
      // CSS animation handles continuous rotation
    },
  },
});
</script>

<style scoped>
/* Container */
.moon-container {
  width: 200px;
  height: 200px;
  margin: 0 auto 36px;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

/* Outer ambient glow */
.moon-glow {
  position: absolute;
  width: 130%;
  height: 130%;
  border-radius: 50%;
  background: radial-gradient(
    circle,
    rgba(220, 220, 160, 0.1) 0%,
    rgba(180, 180, 120, 0.04) 40%,
    transparent 70%
  );
  filter: blur(16px);
  animation: moonPulse 5s infinite ease-in-out;
  pointer-events: none;
}

/* Subtle orbit ring */
.moon-orbit-ring {
  position: absolute;
  width: 115%;
  height: 115%;
  border-radius: 50%;
  border: 1px dashed rgba(255, 255, 220, 0.06);
  pointer-events: none;
  animation: orbitSpin 60s linear infinite;
}

@keyframes orbitSpin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

@keyframes moonPulse {
  0%,
  100% {
    opacity: 0.55;
    transform: scale(1);
  }
  50% {
    opacity: 0.9;
    transform: scale(1.1);
  }
}

/* Moon wrapper */
.moon-wrapper {
  width: 100%;
  height: 100%;
  z-index: 1;
  animation: rotateMoon 180s linear infinite;
  filter: drop-shadow(0 0 20px rgba(220, 215, 170, 0.12));
}

@keyframes rotateMoon {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

/* SVG */
.moon-svg {
  width: 100%;
  height: 100%;
  display: block;
}

/* Desktop */
@media (min-width: 1024px) {
  .moon-container {
    width: 260px;
    height: 260px;
    margin: 0 auto 48px;
  }
}

@media (max-width: 480px) {
  .moon-container {
    width: 175px;
    height: 175px;
    margin-bottom: 28px;
  }
}
</style>
