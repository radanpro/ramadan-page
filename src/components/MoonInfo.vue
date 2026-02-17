<template>
  <div class="info-card">
    <div class="info-item">
      <span class="label">اسم المرحلة</span
      ><span class="value">{{ phaseData.phaseName }}</span>
    </div>
    <div class="info-item">
      <span class="label">نسبة الإضاءة</span
      ><span class="value">{{ phaseData.illumination }}%</span>
    </div>
    <div class="info-item">
      <span class="label">عمر القمر (أيام)</span
      ><span class="value">{{ phaseData.age }} يوم</span>
    </div>
  </div>
</template>

<script>
export default { name: "MoonInfo", props: ["phaseData"] };
</script>
