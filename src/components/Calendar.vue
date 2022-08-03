<template>
  <div>
    <div
      class="border-2 rounded text-left flex justify-between"
      @click="showCalendar = !showCalendar"
    >
      <div>{{ selectedDate }}</div>
      <svg
        xmlns="http://www.w3.org/2000/svg"
        class="h-6 w-6"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
        stroke-width="2"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"
        />
      </svg>
    </div>
    <div v-if="showCalendar" class="bg-gray-200 border-2 rounded-md">
      <div class="flex justify-between">
        <button @click="backMonth()">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-6"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
            stroke-width="2"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M11 19l-7-7 7-7m8 14l-7-7 7-7"
            />
          </svg>
        </button>

        <div class="relative">
          <button class="cursor-pointer" @click="toggleSelectMonth = !toggleSelectMonth">
            <span class="font-bold">{{ monthLabel[month] }} {{ year + 543 }}</span>
          </button>
          <div v-if="toggleSelectMonth" class="absolute bg-white z-1 w-[100px] border-2 border-dashed">
            <div v-for="month of monthLabel" :key="month">
              <button class="hover:text-blue-400" @click="onPickMonth(month)">{{ month }}</button>
            </div>
          </div>
        </div>
        <button @click="nextMonth()">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-5 w-5"
            viewBox="0 0 20 20"
            fill="currentColor"
          >
            <path
              fill-rule="evenodd"
              d="M10.293 15.707a1 1 0 010-1.414L14.586 10l-4.293-4.293a1 1 0 111.414-1.414l5 5a1 1 0 010 1.414l-5 5a1 1 0 01-1.414 0z"
              clip-rule="evenodd"
            />
            <path
              fill-rule="evenodd"
              d="M4.293 15.707a1 1 0 010-1.414L8.586 10 4.293 5.707a1 1 0 011.414-1.414l5 5a1 1 0 010 1.414l-5 5a1 1 0 01-1.414 0z"
              clip-rule="evenodd"
            />
          </svg>
        </button>
      </div>
      <div class="flex">
        <div v-for="dayLabel of daysLabel" :key="dayLabel" class="day">
          <span class="font-bold">{{ dayLabel }}</span>
        </div>
      </div>
      <div class="flex flex-wrap bg-white">
        <div v-for="emptyDay of emptyDays" :key="emptyDay" class="day"></div>
        <div v-for="day of days" :key="day" class="day">
          <button
            @click="onPickDate(day)"
            class="hover:bg-blue-400 rounded px-1 text-sm font-bold"
          >
            {{ day }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Calendar",
  props: {
    msg: String,
  },
  data() {
    const today = new Date();
    const month = today.getMonth();
    const year = today.getFullYear();
    return {
      selectedDate: null,
      showCalendar: true,
      today,
      month,
      year,
      monthLabel: [
        "มกราคม",
        "กุมภาพันธ์",
        "มีนาคม",
        "เมษายน",
        "พฤษภาคม",
        "มิถุนายน",
        "กรกฎาคม",
        "สิงหาคม",
        "กันยายน",
        "ตุลาคม",
        "พฤศจิกายน",
        "ธันวาคม",
      ],
      daysLabel: ["อา.", "จ.", "อ.", "พ.", "พฤ.", "ศ.", "ส."],
      days: [],
      emptyDays: [],
      toggleSelectMonth: false,
    };
  },
  async mounted() {
    this.genDayInMonth();
  },
  methods: {
    backMonth() {
      if (this.month == 0) {
        this.month = 11;
        this.year = this.year - 1;
      } else {
        this.month = this.month - 1;
      }
      this.genDayInMonth();
    },
    nextMonth() {
      if (this.month == 11) {
        this.month = 0;
        this.year = this.year + 1;
      } else {
        this.month = this.month + 1;
      }
      this.genDayInMonth();
    },
    onPickMonth(month) {
        this.month = this.monthLabel.indexOf(month);
        this.genDayInMonth();
        this.toggleSelectMonth = !this.toggleSelectMonth;
    },
    onPickDate(day) {
      const pickDate = new Date(this.year, this.month, day);
      this.selectedDate = pickDate.toLocaleDateString("th-TH", {
        day: "numeric",
        month: "long",
        year: "numeric",
      });
      this.showCalendar = !this.showCalendar;
      alert(this.selectedDate);
    },
    genDayInMonth() {
      this.days = [];
      this.emptyDays = [];
      const allOfDay = new Date(this.year, this.month + 1, 0).getDate();
      const firstDayOfWeek = new Date(this.year, this.month, 1).getDay();
      console.log(firstDayOfWeek);
      for (let i = 1; i <= allOfDay; i++) {
        if (i <= firstDayOfWeek) {
          this.emptyDays.push(i);
        }
        this.days.push(i);
      }
    },
  },
};
</script>

<style>
.day {
  width: 14%;
}
</style>