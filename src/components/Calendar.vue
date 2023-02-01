<script>
let input = null;
export default {
  data() {
    return {
      logo: {
        nameOrg: "calendar-logo.jpg",
        nameNew: "",
      },
      header: {
        name: "桃子老师",
        year: null,
        yearPer: "\u25B2",
        yearNext: "\u25BC",
        month: null,
        monthList: [],
      },
      calendar: {
        weekdays: ["周日", "周一", "周二", "周三", "周四", "周五", "周六"],
        day: [],
      },
      show: {
        showCld: true,
        showIpt: false,
        showYL: false,
        showML: false,
      },
    };
  },
  mounted() {
    this.getCurrentDate();
    this.getDays();
  },
  methods: {
    getCurrentDate() {
      const currentDate = new Date();
      this.header.year = currentDate.getFullYear();
      this.header.month = currentDate.getMonth() + 1;
    },
    getMonthDays(month) {
      let monthDays = 30;
      if ((month < 8 && month % 2 == 1) || (month > 7 && month % 2 == 0)) {
        monthDays = 31;
      } else if (month == 2) {
        const leapYear =
          (year % 4 === 0 && year % 100 !== 0) ||
          (year % 3200 !== 0 && year % 400 === 0);
        monthDays = leapYear ? 29 : 28;
      } else {
        monthDays = 30;
      }
      return monthDays;
    },
    getDays() {
      const year = this.header.year;
      const month = this.header.month - 1;
      const firstWeekday = new Date(year, month).getDay();
      const monthDays = this.getMonthDays(month + 1);
      let day = this.calendar.day;
      day.splice(0, day.length);
      for (let i = 0; i < monthDays + firstWeekday; i++) {
        if (i < firstWeekday) {
          day.push("");
        } else {
          day.push(i - firstWeekday + 1);
        }
      }
      this.getDayNone(firstWeekday);
    },
    getDayNone(firstWeekday) {
      setTimeout(() => {
        const dayList = document.querySelector("#days").children;
        for (let i = 0; i < dayList.length; i++) {
          dayList[i].classList.add("day");
        }
        for (let i = 0; i < firstWeekday; i++) {
          dayList[i].classList.remove("day");
          dayList[i].classList.add("none");
        }
      }, 500);
    },
    getMonthList() {
      let month = this.header.monthList;
      month.splice(0, month.length);
      for (let i = 0; i < 12; i++) {
        month.push(i + 1);
      }
    },
    highLight(e) {
      e.target.classList.toggle("highLight");
    },
    showNone() {
      for (let item in this.show) {
        this.show[item] = false;
      }
    },
    showIpt() {
      if (this.show.showIpt) {
        this.showCld();
      } else {
        this.showNone();
        this.show.showIpt = true;
      }
    },
    showCld() {
      this.showNone();
      this.show.showCld = true;
    },
    showYL() {
      if (this.show.showYL) {
        this.showCld();
      } else {
        this.showNone();
        this.show.showYL = true;
      }
    },
    showML() {
      if (this.show.showML) {
        this.showCld();
      } else {
        this.showNone();
        this.show.showML = true;
      }
    },
    pressEnt(e) {
      const key = e.which || e.keyCode;
      if (key == 13) {
        this.showCld();
      }
    },
    perYear() {
      this.header.year--;
      this.showCld();
      this.getDays();
    },
    nextYear() {
      this.header.year++;
      this.showCld();
      this.getDays();
    },
    newMonth(e) {
      this.header.month = e.target.innerText;
      this.showCld();
      this.getDays();
    },
    reset() {
      this.getCurrentDate();
      this.getDays();
      let day = document.querySelectorAll(".day");
      day.forEach((e) => {
        e.classList.remove("highLight");
      });
    },
    getImg() {
      if (input === null) {
        input = document.createElement("input");
        input.setAttribute("type", "file");
        input.classList.add("uploadIpt");

        if (window.addEventListener) {
          input.addEventListener("change", this.uploadImg, false);
        } else {
          input.attachEvent("onchange", this.uploadImg);
        }
        document.body.appendChild(input);
      }
      input.click();
    },
    uploadImg(el) {
      if (el && el.target && el.target.files && el.target.files.length > 0) {
        const file = el.target.files[0];
        const that = this;
        const reader = new FileReader();
        reader.readAsDataURL(file);
        reader.onload = function () {
          that.logo.nameNew = this.result;

          const img = document.querySelector("#img");
          img.setAttribute("src", `${that.logo.nameNew}`);
          const inputs = document.querySelectorAll(".uploadIpt");
          inputs.forEach((el) => {
            el.remove();
          });
          input = null;
        };
      }
    },
  },
  setup() {
    const getUrl = (name) => {
      return new URL(`../assets/images/${name}`, import.meta.url).href;
    };
    return {
      getUrl,
    };
  },
};
</script>
<template>
  <div id="box">
    <div id="empty"></div>
    <div id="container">
      <div id="logo" @click="getImg">
        <img :src="getUrl(logo.nameOrg)" id="img" />
      </div>

      <div id="header">
        <h2 id="name" @click="showIpt()">{{ header.name }}</h2>
        <h2 id="title">
          <div class="inline">
            <div id="year" @click="showYL()">{{ header.year }}</div>
            <div id="year-list" v-show="show.showYL">
              <div id="year-pre" @click="perYear()">{{ header.yearPer }}</div>
              <div id="year-next" @click="nextYear()">
                {{ header.yearNext }}
              </div>
            </div>
          </div>
          年
          <div class="inline">
            <div id="month" @click="showML(), getMonthList()">
              {{ header.month }}
            </div>
            <div id="month-list" v-show="show.showML">
              <div
                v-for="month in header.monthList"
                :key="month"
                @click="newMonth($event)"
              >
                {{ month }}
              </div>
            </div>
          </div>
          月班表
        </h2>
      </div>

      <div id="content">
        <input
          type="text"
          v-show="show.showIpt"
          id="iptName"
          :placeholder="header.name"
          v-model="header.name"
          @blur="showCld()"
          @keypress="pressEnt($event)"
        />
        <div id="calendar" v-show="show.showCld">
          <div id="week-days">
            <div v-for="weekday in calendar.weekdays" :key="weekday">
              {{ weekday }}
            </div>
          </div>
          <div id="days">
            <div
              v-for="(day, index) in calendar.day"
              :key="index"
              @click="highLight($event)"
              class="day"
            >
              {{ day }}
            </div>
          </div>
        </div>
      </div>

      <div id="reset" @click="reset()" v-show="show.showCld">
        <div>重置</div>
      </div>
    </div>
  </div>
</template>
<style scoped>
#box {
  width: 100vw;
  height: 100vh;
  background: url("../assets/images/calendar-bg.jpg") no-repeat;
  background-size: cover;
}
#empty,
#container {
  background: linear-gradient(
    to right,
    rgba(151, 151, 240, 0.5),
    rgba(255, 201, 215, 0.5)
  );
}
#empty {
  height: var(--btn-size);
}
#container {
  height: calc(100vh - var(--btn-size));
  font-family: var(--ff-albb);
  padding-top: 10vh;
  display: flex;
  flex-direction: column;
  justify-content: start;
  align-items: center;
}
#logo {
  position: relative;
  width: 120px;
  height: 120px;
  border-radius: 50%;
  box-shadow: var(--shadow-purple);
  overflow: hidden;
}
#img {
  width: 100%;
  cursor: pointer;
}
/* ------------------------------------- */
#header {
  margin: 2vh 0;
  text-align: center;
  color: rgba(255, 255, 255, 0.9);
  font-size: 0.8rem;
  text-shadow: var(--shadow-purple);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
#name,
#title {
  flex: 1;
  width: 100%;
  padding: 0.8vh 0.8vh;
}
#name {
  border-bottom: var(--color-lightgrey) 2px solid;
  cursor: pointer;
}
#title {
  position: relative;
}

.inline {
  display: inline-block;
  position: relative;
}
#year,
#month {
  font-size: 1rem;
  cursor: pointer;
  display: inline-block;
  padding: 2px 0;
}
#year,
#year-list {
  width: 60px;
}
#month,
#month-list {
  width: 40px;
}
#year-list,
#month-list {
  position: absolute;
  text-shadow: var(--shadow-white);
}
#month-list div {
  font-size: 1rem;
  cursor: pointer;
}
#name:hover,
#year:hover,
#month:hover,
#year-pre:hover,
#year-next:hover,
#month-list div:hover {
  border-radius: 20px;
  color: var(--color-purple);
  box-shadow: var(--shadow-purple);
  background-color: var(--color-lightgrey);
  transition: all 0.2s ease-in-out;
}

/* ------------------------------------- */
#content {
  position: relative;
}
#iptName {
  border-radius: 20px;
  padding: 10px;
  border: none;
  outline: none;
  background-color: var(--color-lightgrey);
  box-shadow: var(--shadow-purple);
}
#calendar {
  font-weight: bold;
  position: relative;
  height: max-content;
  width: max-content;
  padding: 30px;
  border-radius: 40px;
  box-shadow: var(--shadow-purple);
  text-align: center;
  color: rgba(0, 0, 0, 0.6);
  background-color: rgba(255, 255, 255, 0.8);
}
#week-days,
#days {
  display: grid;
  gap: 15px;
  font-weight: 600;
}
#week-days {
  color: rgba(103, 54, 148, 0.6);
  font-size: 0.9rem;
  grid-template-columns: repeat(7, 1fr);
  border-bottom: var(--color-lightgrey) 1px solid;
  text-shadow: var(--shadow-purple);
}
#days {
  color: rgba(103, 54, 148, 0.6);
  font-size: 0.8rem;
  margin-top: 20px;
  grid-template-columns: repeat(7, 1fr);

  animation: flowdown 0.5s ease-in-out forwards;
  position: relative;
  opacity: 0;
}
.day {
  cursor: pointer;
}
@keyframes flowdown {
  0% {
    opacity: 0;
    transform: translateY(-10px);
  }
  100% {
    opacity: 1;
    transform: none;
  }
}
.day,
#week-days div {
  width: 40px;
  height: 40px;
  border-radius: 50%;
}
.day.highLight {
  color: #fff;
  background-color: rgba(228, 106, 191, 0.4);
  box-shadow: 2px 2px 5px rgba(183, 104, 207, 0.5);
}
.day.highLight::after {
  display: block;
  content: "休息";
  font-size: 0.6rem;
}
.none {
  cursor: unset;
}
#reset {
  margin-top: 2vh;
  padding: 5px 20px;
  border-radius: 15px;
  box-shadow: var(--shadow-purple);
  background-color: rgba(255, 255, 255, 0.8);
  cursor: pointer;
  font-size: 0.8rem;
}
</style>
<style>
.uploadIpt {
  display: none;
}
</style>
