<template>
  <div class="date-input-container">
    <div class="input-wrapper" @click="openPicker">
      <input type="date" v-model="date" ref="dateInput" @change="emitUpdate" />
    </div>
    <button class="update-btn" @click="emitUpdate">تحديث</button>
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
      // Safely try to open the native date picker
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
  margin-bottom: 30px;
  justify-content: center;
  align-items: center;
}

.input-wrapper {
  position: relative;
  display: flex;
  align-items: center;
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 12px;
  padding: 0 12px;
  cursor: pointer;
  transition: 0.3s;
}

.input-wrapper:hover {
  background: rgba(255, 255, 255, 0.15);
  border-color: var(--accent-color);
}

input[type="date"] {
  background: transparent;
  border: none;
  color: white;
  padding: 12px 0;
  font-family: inherit;
  outline: none;
  width: 140px;
  cursor: pointer;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
}

.update-btn {
  background: var(--accent-color);
  color: #000;
  border: none;
  padding: 12px 20px;
  border-radius: 12px;
  font-weight: 700;
  cursor: pointer;
  transition: 0.3s;
  white-space: nowrap;
}

@media (max-width: 480px) {
  .date-input-container {
    flex-direction: column;
    width: 100%;
  }

  .input-wrapper,
  .update-btn {
    width: 100%;
    justify-content: center;
  }
}
</style>
