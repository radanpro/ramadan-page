<template>
  <div v-if="ramadanDay" class="ramadan-daily-container">
    <!-- Header Row -->
    <div class="daily-header">
      <!-- Day badge -->
      <div class="day-badge">
        <span class="day-label-top">ليلة</span>
        <span class="day-number">{{ ramadanDay + 1 }}</span>
        <span class="day-label-bottom">رمضان</span>
      </div>

      <div class="daily-header-text">
        <h2 class="daily-section-title">رسالة اليوم</h2>
        <span class="cat-tag">{{ category }}</span>
      </div>
    </div>

    <!-- Content Card -->
    <div class="content-card">
      <!-- Decorative top accent -->
      <div class="card-accent-line"></div>

      <!-- Icon + quote -->
      <div class="quote-wrapper">
        <div class="quote-icon">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="22"
            height="22"
            viewBox="0 0 24 24"
            fill="currentColor"
          >
            <path
              d="M3.691 6.292C5.094 4.771 7.217 4 10 4h1v2.819l-.804.161c-1.37.274-2.323.813-2.833 1.604A2.902 2.902 0 0 0 6.925 10H10a1 1 0 0 1 1 1v7c0 1.103-.897 2-2 2H3a1 1 0 0 1-1-1v-5l.003-2.919c-.009-.111-.199-2.741 1.688-4.789zM20 20h-6a1 1 0 0 1-1-1v-5l.003-2.919c-.009-.111-.199-2.741 1.688-4.789C16.094 4.771 18.217 4 21 4h1v2.819l-.804.161c-1.37.274-2.323.813-2.833 1.604A2.902 2.902 0 0 0 17.925 10H21a1 1 0 0 1 1 1v7c0 1.103-.897 2-2 2z"
            />
          </svg>
        </div>
        <p class="daily-text">{{ content }}</p>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

interface RamadanContent {
  text: string;
  category: string;
}

export default defineComponent({
  name: "RamadanDaily",
  props: {
    targetDate: {
      type: Date,
      default: () => new Date(),
    },
  },
  data() {
    return {
      ramadanStart: "2026-02-18",
      contentMap: {
        // 1–10: أيام الرحمة
        1: {
          text: "اللهم اجعل هذا الشهر علينا شهر رحمة ومغفرة وبركة.",
          category: "دعاء",
        },
        2: {
          text: "عن أبي هريرة رضي الله عنه، قال رسول الله ﷺ: 'من صام رمضان إيمانًا واحتسابًا غفر له ما تقدم من ذنبه'.  أخرجه الإمامان البخاري ومسلم في صحيحيهما.",
          category: "حديث",
        },
        3: {
          text: "ابدأ الصيام بصفاء النية والاعتماد على الله، واغتنم كل ساعة برحمة منه.",
          category: "تذكير",
        },
        4: {
          text: "من رحم الله عبده في هذه الأيام فقد أفلح، استغفر اليوم وابدأ بداية جديدة.",
          category: "حكمة",
        },
        5: {
          text: "اللهم اجعلنا من عبادك المرحومين في هذا الشهر.",
          category: "دعاء",
        },
        6: {
          text: "عن معاذ بن جبل رضي الله عنه قال: قال رسول الله ﷺ: 'الصدقة تطفئ الخطيئة كما يطفئ الماء النار'.  أخرجه الترمذي (2616)، وابن ماجه (3973)، وأحمد (22068) جميعهم مطولا.",
          category: "حديث",
        },
        7: {
          text: "اليوم خصص وقتك للقرآن والتفكر في معانيه.",
          category: "تذكير",
        },
        8: {
          text: "الصبر والرحمة أساس الصيام، لا تفوت فرصة تعلمهما.",
          category: "حكمة",
        },
        9: {
          text: "اللهم اجعلنا من الذين يغتنمون هذه الأيام بالرحمة والعتق من النار.",
          category: "دعاء",
        },
        10: {
          text: "في الصحيح عن النبي ﷺ أنه قال: إذا دخل رمضان فتحت أبواب الجنة، وغلقت أبواب النار، وسلسلت الشيطان [أخرجه مسلم 2549].",
          category: "حديث",
        },

        // 11–20: أيام المغفرة
        11: {
          text: "اليوم هو بداية أيام المغفرة، اغتنم الوقت بالذكر والدعاء.",
          category: "تذكير",
        },
        12: {
          text: "اللهم اغفر لنا ذنوبنا وارزقنا التقوى في هذه الأيام المباركة.",
          category: "دعاء",
        },
        13: {
          text: "وعن أبي هريرة قال: قال رسول الله صلى الله عليه وسلم: إذا كان أول ليلة من شهر رمضان صفدت الشياطين ومردة الجن، وغلقت أبواب النار فلم يفتح منها باب، وفتحت أبواب الجنة فلم يغلق منها باب، وينادي مناد يا باغي الخير أقبل ويا باغي الشر أقصر، ولله عتقاء من النار وذلك كل ليلة. رواه الترمذي وابن ماجه وصححه الألباني.",
          category: "حديث",
        },
        14: {
          text: "خصص اليوم للصدقات والأعمال الصالحة، فكل عمل فيه مضاعف.",
          category: "تذكير",
        },
        15: {
          text: "اللهم اجعلنا من المغفور لهم، واملأ قلوبنا بالخير والرحمة.",
          category: "دعاء",
        },
        16: {
          text: "عن أبي هريرة رضي الله عنه أن رسول الله صلى الله عليه وسلم قال: «الصيام جُنة فلا يرفث ولا يجهل، وإن امرؤ قاتله أو شاتمه فليقل: إني صائم مرتين» (البخاري ومسلم).",
          category: "حديث",
        },
        17: {
          text: "اليوم ركز على مراجعة نفسك ومحاسبتها لتغتنم المغفرة.",
          category: "تذكير",
        },
        18: {
          text: "اللهم اجعل صيامنا وقيامنا مقبولًا، واغفر لنا ذنوبنا.",
          category: "دعاء",
        },
        19: {
          text: "عن أنس بن مالك رضي الله عنه قال: قال النبي صلى الله عليه وسلم: «تسحَّروا فإن في السحور بركة» (رواه البخاري ومسلم).",
          category: "حديث",
        },
        20: {
          text: "استمر في العبادة والذكر، فالأيام المباركة تتطلب الاجتهاد المستمر.",
          category: "تذكير",
        },

        // 21–30: أيام العتق من النار
        21: {
          text: "اليوم بداية العشر الأواخر، اجعل قيام الليل عادة يومية.",
          category: "تذكير",
        },
        22: {
          text: "اللهم اجعلنا من الذين يعتقون أنفسهم من النار في هذه الأيام المباركة.",
          category: "دعاء",
        },
        23: {
          text: "عن أبي هريرة رضي الله عنه، قال رسول الله ﷺ: 'من قام ليلة القدر إيمانًا واحتسابًا غفر له ما تقدم من ذنبه'. رواه البخاري ومسلم.",
          category: "حديث",
        },
        24: {
          text: "ركز اليوم على الصدقة والإحسان للآخرين، فهي وسيلة للعتق.",
          category: "تذكير",
        },
        25: {
          text: "اللهم اجعل كل ليلة من هذه الليالي لنا مغفرة وعتقًا من النار.",
          category: "دعاء",
        },
        26: {
          text: "عن أبي هريرة رضي الله عنه قال: قال رسول الله صلى الله عليه وسلم: «رغم أنف رجل ذكرت عنده فلم يصل علي، ورغم أنف رجل دخل عليه رمضان ثم انسلخ قبل أن يُغفر له، ورغم أنف رجل أدرك عنده أبواه الكبر فلم يدخلاه الجنة» (رواه الترمذي وصححه الألباني).",
          category: "حديث",
        },
        27: {
          text: "اليوم محتمل أن تكون ليلة القدر، اجعل عبادتك مضاعفة.",
          category: "تذكير",
        },
        28: {
          text: "اغتنم كل ساعة في الليل بالصلاة والذكر، فالجزاء عظيم.",
          category: "حكمة",
        },
        29: {
          text: "اللهم تقبل منا صيامنا وقيامنا وصلواتنا، واغفر لنا ما تقدم من ذنوبنا.",
          category: "دعاء",
        },
        30: {
          text: "اختتم الشهر بعمل صالح وخشوع، واحتسب الأجر العظيم عند الله.",
          category: "تذكير",
        },
      } as Record<number, RamadanContent>,
    };
  },
  computed: {
    formattedDate(): string {
      const d = this.targetDate;
      return `${d.getFullYear()}-${String(d.getMonth() + 1).padStart(2, "0")}-${String(d.getDate()).padStart(2, "0")}`;
    },
    ramadanDay(): number | null {
      const start = new Date(this.ramadanStart + "T00:00:00");
      const current = new Date(this.formattedDate + "T00:00:00");
      const diffTime = current.getTime() - start.getTime();
      const diffDays = Math.floor(diffTime / (1000 * 60 * 60 * 24)) + 1;
      if (diffDays >= 1 && diffDays <= 30) {
        return diffDays;
      }
      return null;
    },
    dailyData(): RamadanContent {
      const day = this.ramadanDay;
      if (day && this.contentMap[day]) {
        return this.contentMap[day];
      }
      return {
        text: "خير الأعمال أدومها وإن قل، اجعل لنفسك خبيئة من عمل صالح.",
        category: "تذكير",
      };
    },
    content(): string {
      return this.dailyData.text;
    },
    category(): string {
      return this.dailyData.category;
    },
  },
});
</script>

<style scoped>
/* Container */
.ramadan-daily-container {
  display: flex;
  flex-direction: column;
  gap: 20px;
  animation: dailyEnter 0.7s cubic-bezier(0.2, 0.8, 0.2, 1) both;
}

@keyframes dailyEnter {
  from {
    opacity: 0;
    transform: translateY(28px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* Header row */
.daily-header {
  display: flex;
  align-items: center;
  gap: 20px;
}

.daily-header-text {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 8px;
}

/* Section title inside daily */
.daily-section-title {
  font-size: 1.5rem !important;
  margin-bottom: 0 !important;
}

/* Day badge */
.day-badge {
  flex-shrink: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 76px;
  height: 76px;
  border-radius: 50%;
  background: conic-gradient(
    from 180deg,
    var(--accent-color, #f5c842) 0%,
    var(--accent-deep, #ca8a04) 100%
  );
  color: #030509;
  box-shadow:
    0 10px 28px rgba(245, 200, 66, 0.38),
    inset 0 1px 0 rgba(255, 255, 255, 0.2);
  border: 3px solid rgba(255, 255, 255, 0.12);
  position: relative;
}

.day-badge::after {
  content: "";
  position: absolute;
  inset: -6px;
  border-radius: 50%;
  border: 1px dashed rgba(245, 200, 66, 0.25);
  animation: badgeSpin 20s linear infinite;
}

@keyframes badgeSpin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

.day-number {
  font-size: 1.85rem;
  font-weight: 900;
  line-height: 1;
}

.day-label-top,
.day-label-bottom {
  font-size: 0.65rem;
  font-weight: 800;
  letter-spacing: 0.04em;
  opacity: 0.85;
}

/* Category tag */
.cat-tag {
  display: inline-flex;
  align-items: center;
  background: rgba(245, 200, 66, 0.1);
  color: var(--accent-color, #f5c842);
  padding: 4px 14px;
  border-radius: 50px;
  font-size: 0.8rem;
  font-weight: 700;
  border: 1px solid rgba(245, 200, 66, 0.2);
  letter-spacing: 0.02em;
}

/* Content card */
.content-card {
  background: linear-gradient(
    145deg,
    rgba(255, 255, 255, 0.05) 0%,
    rgba(255, 255, 255, 0.02) 100%
  );
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 22px;
  padding: 28px 28px 28px;
  width: 100%;
  position: relative;
  overflow: hidden;
  transition:
    transform 0.4s ease,
    border-color 0.4s ease,
    box-shadow 0.4s ease;
}

.content-card:hover {
  transform: translateY(-3px);
  border-color: rgba(245, 200, 66, 0.18);
  box-shadow: 0 16px 40px -12px rgba(0, 0, 0, 0.5);
}

/* Decorative top accent line */
.card-accent-line {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 2px;
  background: linear-gradient(
    90deg,
    transparent,
    rgba(245, 200, 66, 0.5),
    transparent
  );
  border-radius: 2px 2px 0 0;
}

/* Quote area */
.quote-wrapper {
  display: flex;
  align-items: flex-start;
  gap: 16px;
}

.quote-icon {
  flex-shrink: 0;
  width: 40px;
  height: 40px;
  border-radius: 12px;
  background: rgba(245, 200, 66, 0.1);
  border: 1px solid rgba(245, 200, 66, 0.15);
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--accent-color, #f5c842);
  margin-top: 2px;
}

/* Daily text */
.daily-text {
  font-size: 1.1rem;
  line-height: 2;
  color: var(--text-primary, #f1f5f9);
  margin: 0;
  text-align: right;
  font-weight: 400;
}

/* Desktop */
@media (min-width: 1024px) {
  .daily-header {
    flex-direction: row-reverse;
    justify-content: flex-end;
  }

  .daily-header-text {
    align-items: flex-end;
    text-align: right;
  }

  .daily-text {
    text-align: right;
    font-size: 1.15rem;
  }
}

/* Mobile */
@media (max-width: 480px) {
  .content-card {
    padding: 22px 18px;
  }

  .quote-wrapper {
    flex-direction: column;
    gap: 12px;
  }

  .daily-text {
    font-size: 1rem;
    line-height: 1.85;
  }

  .day-badge {
    width: 68px;
    height: 68px;
  }

  .day-number {
    font-size: 1.6rem;
  }
}
</style>
