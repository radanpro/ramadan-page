<template>
  <div class="info-card">
    <div
      class="info-item"
      v-for="(item, index) in displayItems"
      :key="index"
      :style="{ animationDelay: `${index * 0.1}s` }"
    >
      <span class="label">{{ item.label }}</span>
      <span class="value">{{ item.value }}</span>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
  name: "MoonInfo",
  props: {
    phaseData: {
      type: Object as () => Record<string, any> | null,
      required: true,
    },
  },
  computed: {
    displayItems() {
      return [
        { label: "اسم المرحلة", value: this.phaseData?.phaseName },
        { label: "نسبة الإضاءة", value: `${this.phaseData?.illumination}%` },
        { label: "عمر القمر", value: `${this.phaseData?.age} يوم` },
      ];
    },
  },
});
</script>

<style scoped>
.info-card {
  background: rgba(0, 0, 0, 0.25);
  border-radius: 20px;
  padding: 24px;
  border: 1px solid rgba(255, 255, 255, 0.05);
  box-shadow: inset 0 0 20px rgba(0, 0, 0, 0.2);
}

.info-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
  padding-bottom: 12px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.03);
  animation: itemEnter 0.5s ease-out both;
}

.info-item:last-child {
  margin-bottom: 0;
  padding-bottom: 0;
  border-bottom: none;
}

@keyframes itemEnter {
  from {
    opacity: 0;
    transform: translateX(10px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

.label {
  color: var(--text-secondary);
  font-size: 0.95rem;
  font-weight: 500;
}

.value {
  font-weight: 700;
  color: var(--accent-color);
  font-size: 1.05rem;
}

@media (min-width: 1024px) {
  .info-item {
    flex-direction: row-reverse;
  }
}
</style>
