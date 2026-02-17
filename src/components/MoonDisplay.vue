<template>
  <div class="moon-display" ref="moonContainer">
    <div v-html="svgMoon"></div>
  </div>
</template>

<script>
export default {
  name: "MoonDisplay",
  props: ["phase"],
  data() {
    return { svgMoon: "" };
  },
  watch: {
    phase(newVal) {
      this.svgMoon = this.renderMoonSVG(newVal);
    },
  },
  methods: {
    renderMoonSVG(phase) {
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
        <svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
          <defs>
            <radialGradient id="moonSurface" cx="50%" cy="50%" r="50%">
              <stop offset="0%" style="stop-color:#fefce8;stop-opacity:1"/>
              <stop offset="80%" style="stop-color:#facc15;stop-opacity:1"/>
              <stop offset="100%" style="stop-color:#eab308;stop-opacity:1"/>
            </radialGradient>
            <filter id="glow">
              <feGaussianBlur stdDeviation="1.5" result="blur"/>
              <feComposite in="SourceGraphic" in2="blur" operator="over"/>
            </filter>
          </defs>
          <circle cx="50" cy="50" r="48" fill="#111827"/>
          <path d="${path}" fill="url(#moonSurface)" filter="url(#glow)"/>
          <mask id="moonMask"><circle cx="50" cy="50" r="48" fill="white"/></mask>
          <g mask="url(#moonMask)" opacity="0.08" fill="#000">
             <circle cx="35" cy="35" r="6"/>
             <circle cx="65" cy="45" r="8"/>
             <circle cx="45" cy="70" r="4"/>
             <circle cx="25" cy="65" r="3"/>
             <circle cx="75" cy="25" r="5"/>
          </g>
        </svg>
      `;
    },
    animateMoon() {
      const moon = this.$refs.moonContainer;
      moon.style.transform = "scale(1.05)";
      setTimeout(() => (moon.style.transform = "scale(1)"), 200);
    },
  },
};
</script>
