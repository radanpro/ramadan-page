<template>
  <div class="moon-container">
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
              <stop offset="0%" style="stop-color:#fffef0;stop-opacity:1"/>
              <stop offset="60%" style="stop-color:#facc15;stop-opacity:1"/>
              <stop offset="100%" style="stop-color:#ca8a04;stop-opacity:1"/>
            </radialGradient>
            <filter id="moonGlow" x="-50%" y="-50%" width="200%" height="200%">
              <feGaussianBlur stdDeviation="3" result="blur"/>
              <feComposite in="SourceGraphic" in2="blur" operator="over"/>
            </filter>
            <filter id="craterShadow">
              <feDropShadow dx="-0.5" dy="-0.5" stdDeviation="0.5" flood-opacity="0.3"/>
            </filter>
          </defs>
          <circle cx="50" cy="50" r="49" fill="#030712"/>
          <path d="${path}" fill="url(#moonSurface)" filter="url(#moonGlow)"/>
          
          <mask id="moonMask"><circle cx="50" cy="50" r="49" fill="white"/></mask>
          
          <g mask="url(#moonMask)" opacity="0.12" filter="url(#craterShadow)">
             <circle cx="35" cy="35" r="7" fill="#000"/>
             <circle cx="65" cy="45" r="9" fill="#000"/>
             <circle cx="45" cy="70" r="5" fill="#000"/>
             <circle cx="28" cy="62" r="4" fill="#000"/>
             <circle cx="72" cy="28" r="6" fill="#000"/>
             <circle cx="50" cy="20" r="3" fill="#000"/>
          </g>
        </svg>
      `;
    },
    animateMoon() {
      // Animation removed as requested
    },
  },
});
</script>

<style scoped>
.moon-container {
  width: 220px;
  height: 220px;
  margin: 0 auto 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  filter: drop-shadow(0 0 30px rgba(250, 204, 21, 0.15));
}

.moon-wrapper {
  width: 100%;
  height: 100%;
}

.moon-svg {
  width: 100%;
  height: 100%;
  display: block;
}

@media (min-width: 1024px) {
  .moon-container {
    width: 260px;
    height: 260px;
    margin: 0 auto 50px;
  }
}
</style>
