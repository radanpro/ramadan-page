<template>
  <div v-if="ramadanDay" class="ramadan-daily-container">
    <div class="day-badge">
      <span class="day-number">{{ ramadanDay }}</span>
      <span class="day-label">رمضان</span>
    </div>

    <div class="content-card">
      <div class="card-header">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="24"
          height="24"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
          class="title-icon"
        >
          <path
            d="M12 3l1.912 5.886H20.1l-4.994 3.635 1.912 5.886L12 14.772l-5.018 3.635 1.912-5.886-4.994-3.635h6.188z"
          ></path>
        </svg>
        <h3>رسالة اليوم</h3>
      </div>

      <div class="card-body">
        <p class="daily-text">{{ content }}</p>
      </div>

      <div class="card-footer">
        <span class="cat-tag">{{ category }}</span>
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
      // Ramadan 2026 is estimated to start Feb 18 (checking with user instruction Feb 18 = 1 Ramadan)
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
          text: "عن أنس بن مالك رضي الله عنه قال: قال النبي صلى الله عليه وسلم: «تسحَّروا فإن في السحور بركة» (رواه البخاري ومسلم).",
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
      // Use local date part to avoid UTC shifts
      const d = this.targetDate;
      return `${d.getFullYear()}-${String(d.getMonth() + 1).padStart(2, "0")}-${String(d.getDate()).padStart(2, "0")}`;
    },
    ramadanDay(): number | null {
      const start = new Date(this.ramadanStart + "T00:00:00");
      const current = new Date(this.formattedDate + "T00:00:00");

      // Calculate difference in days
      const diffTime = current.getTime() - start.getTime();
      const diffDays = Math.floor(diffTime / (1000 * 60 * 60 * 24)) + 1;

      // Assume Ramadan lasts 30 days
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
.ramadan-daily-container {
  margin-top: 20px;
  animation: slideUp 0.6s ease-out;
  display: flex;
  flex-direction: column;
  gap: 15px;
  align-items: center;
}

@keyframes slideUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.day-badge {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background: linear-gradient(135deg, var(--accent-color), #ca8a04);
  color: #05070a;
  width: 80px;
  height: 80px;
  border-radius: 50%;
  box-shadow: 0 10px 20px rgba(250, 204, 21, 0.3);
  z-index: 2;
  border: 4px solid rgba(255, 255, 255, 0.1);
}

.day-number {
  font-size: 1.8rem;
  font-weight: 800;
  line-height: 1;
}

.day-label {
  font-size: 0.8rem;
  font-weight: 700;
  text-transform: uppercase;
}

.content-card {
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 24px;
  padding: 30px;
  width: 100%;
  position: relative;
  overflow: hidden;
  transition: transform 0.3s ease;
}

.content-card:hover {
  transform: scale(1.02);
  border-color: rgba(250, 204, 21, 0.3);
}

.card-header {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 15px;
  color: var(--accent-color);
}

.title-icon {
  width: 20px;
  height: 20px;
}

h3 {
  margin: 0;
  font-size: 1.1rem;
  font-weight: 700;
}

.daily-text {
  font-size: 1.2rem;
  line-height: 1.7;
  color: #e2e8f0;
  margin: 0;
  text-align: center;
  font-weight: 400;
}

.card-footer {
  margin-top: 20px;
  display: flex;
  justify-content: flex-end;
}

.cat-tag {
  background: rgba(250, 204, 21, 0.1);
  color: var(--accent-color);
  padding: 4px 12px;
  border-radius: 20px;
  font-size: 0.8rem;
  font-weight: 700;
  border: 1px solid rgba(250, 204, 21, 0.2);
}

@media (min-width: 992px) {
  .ramadan-daily-container {
    align-items: flex-end;
  }
  .daily-text {
    text-align: right;
  }
}
</style>
