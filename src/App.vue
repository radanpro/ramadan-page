<template>
  <div class="app-wrapper">
    <div class="container main-layout">
      <!-- Greeting Section -->
      <section class="section-greeting">
        <GreetingCard />
      </section>

      <div class="divider desktop-hidden"></div>

      <!-- Moon Tools Section -->
      <section class="section-moon">
        <h2>مراحل القمر 🌒</h2>
        <DateInput @update="updateMoon" />
        <MoonDisplay :phase="phase" ref="moonDisplay" />
        <MoonInfo v-if="showInfo" :phaseData="phaseData" />
        <RamadanDaily :targetDate="currentDate" />
      </section>
    </div>
  </div>
</template>

<script lang="ts">
import GreetingCard from "@/components/GreetingCard.vue";
import DateInput from "@/components/DateInput.vue";
import MoonDisplay from "@/components/MoonDisplay.vue";
import MoonInfo from "@/components/MoonInfo.vue";
import RamadanDaily from "@/components/RamadanDaily.vue";

export default {
  components: { GreetingCard, DateInput, MoonDisplay, MoonInfo, RamadanDaily },
  data() {
    return {
      phase: 0,
      phaseData: null,
      showInfo: false,
      currentDate: new Date(),
    };
  },
  methods: {
    updateMoon(date) {
      this.currentDate = date;
      const phase = this.calculateMoonPhase(date);
      this.phase = phase;
      this.phaseData = this.getPhaseData(phase);
      this.showInfo = true;
      (this.$refs.moonDisplay as any).animateMoon();
    },
    calculateMoonPhase(date) {
      const year = date.getFullYear();
      let month = date.getMonth() + 1;
      let day = date.getDate();
      let lYear = year;
      if (month < 3) {
        lYear--;
        month += 12;
      }
      const A = Math.floor(lYear / 100);
      const B = Math.floor(A / 4);
      const C = 2 - A + B;
      const E = Math.floor(365.25 * (lYear + 4716));
      const F = Math.floor(30.6001 * (month + 1));
      const jd = C + day + E + F - 1524.5;
      const daysSinceNew = jd - 2451549.5;
      const newMoons = daysSinceNew / 29.53058867;
      return newMoons - Math.floor(newMoons);
    },
    getPhaseData(phase) {
      const phaseName =
        phase < 0.03 || phase > 0.97
          ? "محاق (New Moon)"
          : phase < 0.22
            ? "هلال متزايد (Waxing Crescent)"
            : phase < 0.28
              ? "تربيع أول (First Quarter)"
              : phase < 0.47
                ? "أحدب متزايد (Waxing Gibbous)"
                : phase < 0.53
                  ? "بدر (Full Moon)"
                  : phase < 0.72
                    ? "أحدب متناقص (Waning Gibbous)"
                    : phase < 0.78
                      ? "تربيع أخير (Last Quarter)"
                      : "هلال متناقص (Waning Crescent)";
      const illumination = (
        (Math.abs(Math.cos(phase * 2 * Math.PI) - 1) / 2) *
        100
      ).toFixed(1);
      const age = (phase * 29.53).toFixed(1);
      return { phaseName, illumination, age };
    },
  },
  mounted() {
    this.updateMoon(new Date());
  },
};
</script>

<style></style>
