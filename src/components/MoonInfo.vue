<template>
  <div class="info-card">
    <div
      class="info-item"
      v-for="(item, index) in displayItems"
      :key="index"
      :style="{ animationDelay: `${index * 0.12}s` }"
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
/* Card */
.info-card {
  background: rgba(0, 0, 0, 0.2);
  border-radius: 18px;
  padding: 20px 22px;
  border: 1px solid rgba(255, 255, 255, 0.05);
  box-shadow:
    inset 0 0 30px rgba(0, 0, 0, 0.15),
    0 4px 16px rgba(0, 0, 0, 0.2);
  display: flex;
  flex-direction: column;
  gap: 0;
}

/* Row */
.info-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 0;
  border-bottom: 1px solid rgba(255, 255, 255, 0.04);
  animation: itemEnter 0.45s ease-out both;
  gap: 12px;
}

.info-item:last-child {
  padding-bottom: 0;
  border-bottom: none;
}

.info-item:first-child {
  padding-top: 0;
}

@keyframes itemEnter {
  from {
    opacity: 0;
    transform: translateX(14px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

/* Label */
.label {
  color: var(--text-secondary, #94a3b8);
  font-size: 0.88rem;
  font-weight: 500;
  white-space: nowrap;
}

/* Value */
.value {
  font-weight: 700;
  color: var(--accent-color, #f5c842);
  font-size: 0.95rem;
  text-align: left;
  background: linear-gradient(
    135deg,
    var(--accent-color, #f5c842) 0%,
    var(--accent-hover, #fde047) 100%
  );
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

/* Desktop */
@media (min-width: 1024px) {
  .info-item {
    flex-direction: row-reverse;
  }

  .value {
    text-align: right;
  }
}
</style>
