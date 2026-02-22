<template>
  <div class="greeting-content">
    <!-- Image Frame -->
    <div class="image-frame">
      <img
        src="../assets/images/ramadan.jpg"
        alt="رمضان كريم"
        class="greeting-img"
        loading="eager"
      />
      <div class="image-overlay"></div>
      <div class="image-badge">رمضان كريم ✨</div>
    </div>

    <!-- Text Block -->
    <div class="greeting-text">
      <h1 class="greeting-title">رمضان مبارك</h1>
      <p class="greeting-desc">
        أهنئكم بحلول شهر رمضان المبارك، سائلين الله أن يتقبل منا ومنكم صالح
        الأعمال، وأن يعيده علينا وعليكم باليُمن والبركات.
      </p>
      <div class="signature-container">
        <div class="signature-block">
          <span class="signature">م. عبدالرحمن رعدان</span>
          <span class="signature-sub">مطور ومصمم</span>
        </div>
        <span class="signature-line"></span>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({ name: "GreetingCard" });
</script>

<style scoped>
/* Wrapper */
.greeting-content {
  display: flex;
  flex-direction: column;
  gap: 28px;
  animation: greetingEnter 0.9s cubic-bezier(0.2, 0.8, 0.2, 1) both;
}

@keyframes greetingEnter {
  from {
    opacity: 0;
    transform: translateY(24px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* Image */
.image-frame {
  position: relative;
  width: 100%;
  border-radius: 22px;
  overflow: hidden;
  box-shadow:
    0 24px 48px -12px rgba(0, 0, 0, 0.65),
    0 0 0 1px rgba(255, 255, 255, 0.05) inset;
  border: 1px solid rgba(255, 255, 255, 0.07);
}

.greeting-img {
  width: 100%;
  height: auto;
  display: block;
  object-fit: cover;
  transition: transform 1.4s cubic-bezier(0.2, 0.8, 0.2, 1);
}

.image-frame:hover .greeting-img {
  transform: scale(1.06);
}

.image-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(
    to top,
    rgba(3, 5, 9, 0.65) 0%,
    rgba(3, 5, 9, 0.15) 40%,
    transparent 65%
  );
  pointer-events: none;
}

/* Badge on image */
.image-badge {
  position: absolute;
  bottom: 16px;
  right: 16px;
  background: rgba(10, 14, 25, 0.75);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border: 1px solid rgba(245, 200, 66, 0.2);
  color: var(--accent-color, #f5c842);
  font-size: 0.78rem;
  font-weight: 700;
  padding: 6px 14px;
  border-radius: 50px;
  pointer-events: none;
  letter-spacing: 0.03em;
}

/* Text block */
.greeting-text {
  display: flex;
  flex-direction: column;
  gap: 14px;
}

/* Title */
.greeting-title {
  font-size: 2.8rem;
  font-weight: 900;
  color: var(--accent-color, #f5c842);
  text-shadow:
    0 0 40px rgba(245, 200, 66, 0.25),
    0 4px 16px rgba(245, 200, 66, 0.15);
  letter-spacing: -0.01em;
  line-height: 1.15;
  margin: 0;
}

/* Description */
.greeting-desc {
  font-size: 1.05rem;
  line-height: 2.1;
  color: var(--text-secondary, #94a3b8);
  font-weight: 400;
  max-width: 95%;
  margin: 0;
}

/* Signature */
.signature-container {
  display: flex;
  align-items: center;
  gap: 16px;
  margin-top: 8px;
}

.signature-line {
  flex: 1;
  height: 1px;
  background: linear-gradient(
    to right,
    var(--accent-color, #f5c842),
    transparent
  );
  border-radius: 2px;
  max-width: 60px;
}

.signature-block {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 2px;
}

.signature {
  font-weight: 800;
  font-size: 1rem;
  color: var(--text-primary, #f1f5f9);
  letter-spacing: 0.01em;
}

.signature-sub {
  font-size: 0.78rem;
  color: var(--text-secondary, #94a3b8);
  font-weight: 400;
}

/* Desktop */
@media (min-width: 1024px) {
  .greeting-content {
    text-align: right;
  }

  .greeting-desc {
    margin-right: 0;
    margin-left: auto;
  }

  .signature-container {
    flex-direction: row-reverse;
    justify-content: flex-end;
  }

  .signature-line {
    background: linear-gradient(
      to left,
      var(--accent-color, #f5c842),
      transparent
    );
  }
}

/* Mobile */
@media (max-width: 480px) {
  .greeting-title {
    font-size: 2.2rem;
  }

  .greeting-desc {
    font-size: 0.98rem;
  }
}
</style>
