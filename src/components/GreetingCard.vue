<template>
  <div class="greeting-content">
    <div class="image-frame">
      <img
        src="../assets/images/ramadan.jpg"
        alt="Ramadan Mubarak"
        class="greeting-img"
      />
      <div class="image-overlay"></div>
      <div class="glow-effect"></div>
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
  gap: 30px;
}

.image-frame {
  position: relative;
  width: 100%;
  border-radius: 28px;
  overflow: hidden;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.5);
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.greeting-img {
  width: 100%;
  height: auto;
  display: block;
  transition: transform 1.2s var(--ease-premium, cubic-bezier(0.2, 0.8, 0.2, 1));
}

.image-frame:hover .greeting-img {
  transform: scale(1.08);
}

.image-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(to top, rgba(2, 4, 8, 0.5) 0%, transparent 50%);
  pointer-events: none;
}

.glow-effect {
  position: absolute;
  inset: -1px;
  border-radius: 28px;
  box-shadow: inset 0 0 40px rgba(250, 204, 21, 0.1);
  pointer-events: none;
}

.greeting-text {
  animation: slideUpFade 0.8s
    var(--ease-premium, cubic-bezier(0.2, 0.8, 0.2, 1)) both;
}

@keyframes slideUpFade {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.greeting-title {
  font-size: 3rem;
  font-weight: 800;
  margin: 0 0 20px;
  color: var(--accent-color);
  text-shadow: 0 4px 20px rgba(250, 204, 21, 0.3);
  letter-spacing: -0.02em;
}

.greeting-desc {
  font-size: 1.15rem;
  line-height: 2;
  color: var(--text-secondary);
  margin-bottom: 35px;
  font-weight: 400;
  max-width: 90%;
}

.signature-container {
  display: flex;
  align-items: center;
  gap: 15px;
  justify-content: flex-end;
}

.signature-line {
  height: 2px;
  width: 50px;
  background: linear-gradient(to left, var(--accent-color), transparent);
  border-radius: 2px;
}

.signature {
  font-weight: 700;
  font-size: 1.3rem;
  color: var(--accent-color);
  letter-spacing: 0.02em;
}

@media (min-width: 1024px) {
  .greeting-content {
    text-align: right;
  }
  .greeting-desc {
    margin-left: 0;
    margin-right: auto;
  }
  .signature-container {
    justify-content: flex-start;
    flex-direction: row-reverse;
  }
  .signature-line {
    background: linear-gradient(to right, var(--accent-color), transparent);
  }
}
</style>
