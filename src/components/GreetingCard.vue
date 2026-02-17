<template>
  <div class="greeting-content">
    <div class="image-frame">
      <img
        src="../assets/images/ramadan.jpg"
        alt="Ramadan Mubarak"
        class="greeting-img"
      />
      <div class="image-overlay"></div>
    </div>
    <div class="greeting-text">
      <h1 class="greeting-title">رمضان مبارك</h1>
      <p class="greeting-desc">
        أهنئكم بحلول شهر رمضان المبارك، سائلين الله أن يتقبل منا ومنكم صالح
        الأعمال، وأن يعيده علينا وعليكم باليمن والبركات.
      </p>
      <div class="signature-container">
        <span class="signature-line"></span>
        <span class="signature">م. عبدالرحمن رعدان</span>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({ name: "GreetingCard" });
</script>

<style scoped>
.greeting-content {
  display: flex;
  flex-direction: column;
  gap: 25px;
}

.image-frame {
  position: relative;
  width: 100%;
  border-radius: 24px;
  overflow: hidden;
  box-shadow: 0 15px 35px rgba(0, 0, 0, 0.4);
}

.greeting-img {
  width: 100%;
  height: auto;
  display: block;
  transition: transform 0.8s cubic-bezier(0.2, 0.8, 0.2, 1);
}

.image-frame:hover .greeting-img {
  transform: scale(1.05);
}

.image-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(to top, rgba(5, 7, 10, 0.4), transparent);
}

.greeting-title {
  font-size: 2.2rem;
  font-weight: 800;
  margin: 0 0 15px;
  color: var(--accent-color);
  text-shadow: 0 2px 10px rgba(250, 204, 21, 0.2);
}

.greeting-desc {
  font-size: 1.1rem;
  line-height: 1.8;
  color: var(--text-secondary);
  margin-bottom: 30px;
}

.signature-container {
  display: flex;
  align-items: center;
  gap: 15px;
  justify-content: flex-end;
}

.signature-line {
  height: 2px;
  width: 40px;
  background: var(--accent-color);
  opacity: 0.5;
}

.signature {
  font-weight: 700;
  font-size: 1.2rem;
  color: var(--accent-color);
}

@media (min-width: 1024px) {
  .greeting-content {
    text-align: right;
  }
  .signature-container {
    justify-content: flex-start;
    flex-direction: row-reverse;
  }
}
</style>
