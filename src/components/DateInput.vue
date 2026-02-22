<template>
  <div class="date-input-container">
    <div class="input-wrapper" @click="openPicker">
      <input type="date" v-model="date" ref="dateInput" @change="emitUpdate" />
      <span class="placeholder-text" v-if="!date">اختر التاريخ</span>
      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="18"
        height="18"
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        stroke-width="2"
        stroke-linecap="round"
        stroke-linejoin="round"
        class="calendar-icon"
      >
        <rect x="3" y="4" width="18" height="18" rx="2" ry="2"></rect>
        <line x1="16" y1="2" x2="16" y2="6"></line>
        <line x1="8" y1="2" x2="8" y2="6"></line>
        <line x1="3" y1="10" x2="21" y2="10"></line>
      </svg>
    </div>
    <button class="update-btn" @click="emitUpdate">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="18"
        height="18"
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        stroke-width="2.5"
        stroke-linecap="round"
        stroke-linejoin="round"
        class="refresh-icon"
      >
        <path d="M21 12a9 9 0 1 1-9-9c2.52 0 4.93 1 6.74 2.74L21 8"></path>
        <path d="M21 3v5h-5"></path>
      </svg>
      تحديث
    </button>
  </div>
</template>

<script lang="ts">
export default {
  name: "DateInput",
  data() {
    const now = new Date();
    const localDate = `${now.getFullYear()}-${String(now.getMonth() + 1).padStart(2, "0")}-${String(now.getDate()).padStart(2, "0")}`;
    return {
      date: localDate,
    };
  },

  methods: {
    emitUpdate() {
      this.$emit("update", new Date(this.date));
    },
    openPicker() {
      const input = this.$refs.dateInput as HTMLInputElement;
      if (input && typeof input.showPicker === "function") {
        input.showPicker();
      } else if (input) {
        input.focus();
      }
    },
  },
};
</script>

<style scoped>
.date-input-container {
  display: flex;
  gap: 16px;
  margin-bottom: 40px;
  justify-content: center;
  align-items: stretch;
}

.input-wrapper {
  position: relative;
  display: flex;
  align-items: center;
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid var(--glass-border);
  border-radius: 18px;
  padding: 0 20px;
  cursor: pointer;
  transition: all 0.4s var(--ease-premium, cubic-bezier(0.2, 0.8, 0.2, 1));
  box-shadow: inset 0 0 20px rgba(255, 255, 255, 0.01);
}

.input-wrapper:hover {
  background: rgba(255, 255, 255, 0.06);
  border-color: rgba(250, 204, 21, 0.3);
  transform: translateY(-2px);
  box-shadow: 0 10px 25px -10px rgba(0, 0, 0, 0.4);
}

.input-wrapper:focus-within {
  border-color: var(--accent-color);
  background: rgba(255, 255, 255, 0.08);
  box-shadow: 0 0 0 3px rgba(250, 204, 21, 0.1);
}

.calendar-icon {
  margin-right: 12px;
  color: var(--text-secondary);
  transition: color 0.3s ease;
}

.input-wrapper:hover .calendar-icon {
  color: var(--accent-color);
}

input[type="date"] {
  background: transparent;
  border: none;
  color: var(--accent-color);
  padding: 14px 0;
  font-family: inherit;
  font-size: 1.1rem;
  font-weight: 700;
  outline: none;
  width: 150px;
  cursor: pointer;
  appearance: none;
  -webkit-appearance: none;
  text-align: center;
  order: 2;
}

input[type="date"]::-webkit-calendar-picker-indicator {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  width: auto;
  height: auto;
  color: transparent;
  background: transparent;
  cursor: pointer;
}

.update-btn {
  background: linear-gradient(135deg, var(--accent-color) 0%, #eab308 100%);
  color: #020408;
  border: none;
  padding: 0 32px;
  border-radius: 18px;
  font-weight: 800;
  font-size: 1.05rem;
  cursor: pointer;
  transition: all 0.4s var(--ease-premium, cubic-bezier(0.2, 0.8, 0.2, 1));
  display: flex;
  align-items: center;
  gap: 12px;
  box-shadow: 0 8px 15px -5px rgba(250, 204, 21, 0.3);
}

.update-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 15px 30px -8px rgba(250, 204, 21, 0.5);
  filter: brightness(1.05);
}

.update-btn:active {
  transform: translateY(0) scale(0.96);
}

.refresh-icon {
  transition: transform 0.8s var(--ease-premium, cubic-bezier(0.2, 0.8, 0.2, 1));
}

.update-btn:hover .refresh-icon {
  transform: rotate(180deg);
}

@media (max-width: 480px) {
  .date-input-container {
    flex-direction: column;
    gap: 12px;
  }

  .input-wrapper,
  .update-btn {
    width: 100%;
    justify-content: center;
    padding: 12px 20px;
  }

  input[type="date"] {
    width: 100%;
    font-size: 1.15rem;
  }
}
</style>
