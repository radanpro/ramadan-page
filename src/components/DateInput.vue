<template>
  <div class="input-group">
    <input type="date" v-model="date" />
    <button @click="emitUpdate">عرض المرحلة</button>
  </div>
</template>

<script>
export default {
  name: "DateInput",
  data() {
    return { date: new Date().toISOString().substr(0, 10) };
  },
  methods: {
    emitUpdate() {
      this.$emit("update", new Date(this.date));
    },
  },
};
</script>
