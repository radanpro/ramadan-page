<template>
  <div class="date-input-container">
    <div class="input-wrapper" @click="openPicker">
      <!-- Calendar icon -->
      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="16"
        height="16"
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
      <input
        type="date"
        v-model="date"
        ref="dateInput"
        @change="emitUpdate"
        id="date-picker-input"
      />
    </div>

    <button class="update-btn" @click="emitUpdate" id="update-moon-btn">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="16"
        height="16"
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
      <span>تحديث</span>
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
  gap: 12px;
  margin-bottom: 36px;
  align-items: stretch;
}

/* Input wrapper */
.input-wrapper {
  flex: 1;
  position: relative;
  display: flex;
  align-items: center;
  gap: 10px;
  background: rgba(255, 255, 255, 0.035);
  border: 1px solid var(--glass-border, rgba(255, 255, 255, 0.07));
  border-radius: 16px;
  padding: 0 16px;
  cursor: pointer;
  transition:
    background 0.35s ease,
    border-color 0.35s ease,
    transform 0.35s ease,
    box-shadow 0.35s ease;
  min-height: 50px;
}

.input-wrapper:hover {
  background: rgba(255, 255, 255, 0.06);
  border-color: rgba(245, 200, 66, 0.28);
  transform: translateY(-2px);
  box-shadow: 0 8px 24px -8px rgba(0, 0, 0, 0.35);
}

.input-wrapper:focus-within {
  border-color: var(--accent-color, #f5c842);
  background: rgba(255, 255, 255, 0.06);
  box-shadow: 0 0 0 3px rgba(245, 200, 66, 0.1);
}

/* Calendar icon */
.calendar-icon {
  color: var(--text-muted, #475569);
  transition: color 0.3s ease;
  flex-shrink: 0;
}

.input-wrapper:hover .calendar-icon,
.input-wrapper:focus-within .calendar-icon {
  color: var(--accent-color, #f5c842);
}

/* Date input */
input[type="date"] {
  flex: 1;
  background: transparent;
  border: none;
  color: var(--accent-color, #f5c842);
  padding: 12px 0;
  font-family: "Cairo", sans-serif;
  font-size: 1rem;
  font-weight: 700;
  outline: none;
  cursor: pointer;
  appearance: none;
  -webkit-appearance: none;
  text-align: right;
  min-width: 0;
}

input[type="date"]::-webkit-calendar-picker-indicator {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  color: transparent;
  background: transparent;
  cursor: pointer;
  opacity: 0;
}

/* Update button */
.update-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  background: linear-gradient(
    135deg,
    var(--accent-color, #f5c842) 0%,
    var(--accent-deep, #ca8a04) 100%
  );
  color: #03050a;
  border: none;
  padding: 0 24px;
  border-radius: 16px;
  font-weight: 800;
  font-size: 0.95rem;
  font-family: "Cairo", sans-serif;
  cursor: pointer;
  transition:
    transform 0.35s cubic-bezier(0.2, 0.8, 0.2, 1),
    box-shadow 0.35s ease,
    filter 0.25s ease;
  box-shadow: 0 6px 18px -4px rgba(245, 200, 66, 0.35);
  white-space: nowrap;
  min-height: 50px;
}

.update-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 12px 28px -6px rgba(245, 200, 66, 0.55);
  filter: brightness(1.06);
}

.update-btn:active {
  transform: translateY(0) scale(0.96);
  box-shadow: 0 4px 10px -3px rgba(245, 200, 66, 0.3);
}

/* Refresh icon animation */
.refresh-icon {
  transition: transform 0.7s cubic-bezier(0.2, 0.8, 0.2, 1);
  flex-shrink: 0;
}

.update-btn:hover .refresh-icon {
  transform: rotate(180deg);
}

/* Mobile */
@media (max-width: 440px) {
  .date-input-container {
    flex-direction: column;
    gap: 10px;
  }

  .input-wrapper {
    justify-content: center;
    padding: 10px 16px;
  }

  input[type="date"] {
    text-align: center;
    font-size: 1.05rem;
  }

  .update-btn {
    padding: 14px 20px;
    justify-content: center;
    font-size: 1rem;
  }
}
</style>
