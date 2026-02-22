<template>
  <div class="moon-container">
    <div class="moon-glow"></div>
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
            <radialGradient id="moonSurface" cx="40%" cy="40%" r="60%">
              <stop offset="0%" style="stop-color:#F8F8F2;stop-opacity:1"/>
              <stop offset="60%" style="stop-color:#E2E2D3;stop-opacity:1"/>
              <stop offset="100%" style="stop-color:#D6D6C8;stop-opacity:1"/>
            </radialGradient>
            <filter id="moonInnerShadow">
              <feComponentTransfer in=SourceAlpha>
                <feFuncA type="table" tableValues="1 0" />
              </feComponentTransfer>
              <feGaussianBlur stdDeviation="3"/>
              <feOffset dx="-2" dy="-2" result="offsetblur"/>
              <feFlood flood-color="black" flood-opacity="0.4" result="color"/>
              <feComposite in2="offsetblur" operator="in"/>
              <feComposite in2="SourceAlpha" operator="in" />
              <feMerge>
                <feMergeNode in="SourceGraphic" />
                <feMergeNode />
              </feMerge>
            </filter>
            <mask id="moonMask"><circle cx="50" cy="50" r="49" fill="white"/></mask>
          </defs>
          <circle cx="50" cy="50" r="49" fill="#05070a"/>
          <path d="${path}" fill="url(#moonSurface)" filter="url(#moonInnerShadow)"/>
          
          <g mask="url(#moonMask)" opacity="0.1" style="transform-origin: 50% 50%; transform: rotate(15deg);">
             <circle cx="35" cy="35" r="7" fill="#000"/>
             <circle cx="65" cy="45" r="9" fill="#000"/>
             <circle cx="45" cy="70" r="5" fill="#000"/>
             <circle cx="28" cy="62" r="4" fill="#000"/>
             <circle cx="72" cy="28" r="6" fill="#000"/>
             <circle cx="50" cy="20" r="3" fill="#000"/>
             <circle cx="15" cy="45" r="4" fill="#000"/>
             <circle cx="85" cy="55" r="3" fill="#000"/>
          </g>
        </svg>
      `;
    },
    animateMoon() {
      // Internal SVG update triggers re-render,
      // added CSS animation below for continuous rotation
    },
  },
});
</script>

<style scoped>
.moon-container {
  width: 220px;
  height: 220px;
  margin: 0 auto 45px;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.moon-glow {
  position: absolute;
  width: 100%;
  height: 100%;
  border-radius: 50%;
  background: radial-gradient(
    circle,
    rgba(255, 255, 255, 0.1) 0%,
    transparent 70%
  );
  filter: blur(20px);
  animation: pulse 4s infinite ease-in-out;
}

@keyframes pulse {
  0%,
  100% {
    opacity: 0.5;
    transform: scale(1);
  }
  50% {
    opacity: 0.8;
    transform: scale(1.1);
  }
}

.moon-wrapper {
  width: 100%;
  height: 100%;
  z-index: 1;
  animation: rotateMoon 120s linear infinite;
}

@keyframes rotateMoon {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

.moon-svg {
  width: 100%;
  height: 100%;
  display: block;
  filter: drop-shadow(0 0 15px rgba(255, 255, 255, 0.1));
}

@media (min-width: 1024px) {
  .moon-container {
    width: 280px;
    height: 280px;
    margin: 0 auto 60px;
  }
}
</style>
