<template>
  <div class="date-input-container">
    <div class="input-wrapper" @click="openPicker">
      <input type="date" v-model="date" ref="dateInput" @change="emitUpdate" />
      <span class="placeholder-text" v-if="!date">اختر التاريخ</span>
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
    return {
      date: new Date().toISOString().substring(0, 10),
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
  margin-bottom: 35px;
  justify-content: center;
  align-items: stretch;
}

.input-wrapper {
  position: relative;
  display: flex;
  align-items: center;
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 16px;
  padding: 0 20px;
  cursor: pointer;
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: inset 0 0 20px rgba(255, 255, 255, 0.02);
}

.input-wrapper:hover {
  background: rgba(255, 255, 255, 0.07);
  border-color: rgba(250, 204, 21, 0.4);
  transform: translateY(-1px);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
}

.input-wrapper:focus-within {
  border-color: var(--accent-color);
  background: rgba(255, 255, 255, 0.1);
  box-shadow: 0 0 0 3px rgba(250, 204, 21, 0.1);
}

input[type="date"] {
  background: transparent;
  border: none;
  color: var(--accent-color);
  padding: 14px 0;
  font-family: inherit;
  font-size: 1.1rem;
  font-weight: 600;
  outline: none;
  width: 150px;
  cursor: pointer;
  appearance: none;
  -webkit-appearance: none;
  text-align: center;
}

/* Chrome, Safari, Edge */
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
  color: #05070a;
  border: none;
  padding: 0 28px;
  border-radius: 16px;
  font-weight: 800;
  font-size: 1rem;
  cursor: pointer;
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  display: flex;
  align-items: center;
  gap: 10px;
  box-shadow: 0 4px 15px rgba(250, 204, 21, 0.2);
}

.update-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 25px rgba(250, 204, 21, 0.4);
  filter: brightness(1.1);
}

.update-btn:active {
  transform: translateY(0) scale(0.97);
}

.refresh-icon {
  transition: transform 0.6s ease;
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
    padding: 14px 20px;
  }

  input[type="date"] {
    width: 100%;
    font-size: 1.2rem;
  }
}
</style>
