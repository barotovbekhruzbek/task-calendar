<template>
  <div id="app">
  
    
  
    
    <div class="calendar-cont">
      <div class="calendar-header">
        <button type="button" class="button" @click="setToday">
          Hozirgi oy
        </button>
        <button type="button" class="button button-icon" @click="prevMonth">
          <span>&#10094;</span>
        </button>
        <button type="button" class="button button-icon" @click="nextMonth">
          <span>&#10095;</span>
        </button>
        <h4 class="ml-4 mb-0">{{ monthName }} {{ year }}</h4>
      </div>
      <div class="calendar-body">
        <ul class="daynames">
          <li v-for="(dow, d) in dayNames" :key="`dow${d}`">{{ dow }}</li>
        </ul>
        <ul class="dates month">
          <li v-for="e in emptyStartMonth" :key="`nonedate${e}`"></li>
          <li
            v-for="day in daysInMonth"
            @click="selectDate(day)"
            :key="`day${day}`"
            :class="`${
              day == initialDate && month == initialMonth && year == initialYear
                ? 'today'
                : ''
            } ${isselected(day)}`"
          >
            {{ day }}
          </li>
        </ul>
      </div>
      <div class="save">
        <button type="button" class="btn" data-bs-toggle="modal" data-bs-target="#exampleModal">Сохранить</button>
        
   
      </div>

      <!-- Button trigger modal -->


<!-- Modal -->
<div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h1 class="modal-title fs-5" id="exampleModalLabel">Siz shuni belgilamoqchimisiz</h1>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        Agar ha ni bossangiz shu kunda siz bilan suxbat bo`lib o'tadi
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">yoq</button>
        <button type="button" class="btn btn-primary">ha </button>
      </div>
    </div>
  </div>
</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dateContext: new Date(),
      today: new Date(),
      selected: null,
      dayNames: [
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
        "Sunday"
      ]
    };
  },
  computed: {
    selectedBetween() {
      const dates = [];
      if (
        this.selected &&
        Array.isArray(this.selected) &&
        this.selected.length === 2
      ) {
        const startDate =
          new Date(this.selected[0]) > new Date(this.selected[1])
            ? new Date(this.selected[1])
            : new Date(this.selected[0]);
        const endDate =
          new Date(this.selected[0]) > new Date(this.selected[1])
            ? new Date(this.selected[0])
            : new Date(this.selected[1]);

        const currentDate = startDate;
        while (currentDate < endDate) {
          dates.push(this.formatDate(currentDate));
          currentDate.setDate(currentDate.getDate() + 1);
        }
        dates.push(this.formatDate(endDate));
      }
      return dates;
    },
    monthName() {
      const months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December"
      ];
      return months[this.dateContext.getMonth()];
    },
    month() {
      return this.dateContext.getMonth() + 1;
    },
    year() {
      return this.dateContext.getFullYear();
    },
    daysInMonth() {
      const month = this.month || new Date().getMonth() + 1;
      const year = this.year || new Date().getFullYear();
      const daysCount = new Date(year, month, 0).getDate();
      const daysArr = Array.from({ length: daysCount }, (v, k) => k + 1);
      return daysArr;
    },
    initialDate() {
      return this.today.getDate();
    },
    initialMonth() {
      return this.today.getMonth() + 1;
    },
    initialYear() {
      return this.today.getFullYear();
    },
    emptyStartMonth() {
      const nums = new Date(this.year, this.month - 1, 0).getDay();
      const daysArr = Array.from({ length: nums }, (v, k) => k + 1);
      return daysArr;
    }
  },
  methods: {
    selectDate(day) {
      if (
        this.selected &&
        Array.isArray(this.selected) &&
        this.selected.length === 2
      ) {
        this.selected = null;
        this.pushDate(day);
      } else {
        this.pushDate(day);
      }
    },
    pushDate(day) {
      const dateContext = new Date(this.dateContext);
      dateContext.setDate(day);
      if (!this.selected) {
        this.selected = [];
      }
      this.selected.push(dateContext);
    },
    isselected(day) {
      const dateC = new Date(this.dateContext);
      dateC.setDate(day);
      const cssClass = this.selectedBetween.includes(this.formatDate(dateC))
        ? "selected"
        : "d";

      return cssClass;
    },
    formatDate(date) {
      const dateC = new Date(date);
      return `${dateC.getFullYear()}-${
        dateC.getMonth() + 1
      }-${dateC.getDate()}`;
    },
    setToday() {
      this.dateContext = new Date();
    },
    nextMonth() {
      const dateCopy = new Date(this.dateContext.getTime());
      dateCopy.setMonth(dateCopy.getMonth() + 1);
      this.dateContext = dateCopy;
    },
    prevMonth() {
      const dateCopy = new Date(this.dateContext.getTime());
      dateCopy.setMonth(dateCopy.getMonth() - 1);
      this.dateContext = dateCopy;
    }
  }
};
</script>

<!-- Use preprocessors via the lang attribute! e.g. <style lang="scss"> -->
<style>
html,
body {
  font-size: 100%;
  margin: 0;
  padding: 0;
  -webkit-text-size-adjust: 100%;
  -ms-text-size-adjust: 100%;
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
#app {
  max-width: 800px;
  margin: 0 auto;
}
.button {
  border: 0;
  vertical-align: baseline;
  text-transform: none;
  cursor: pointer;
  -webkit-user-select: none;
  -moz-user-select: none;
  user-select: none;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  border-radius: 4px;
  padding: 0 2em;
  background-color: tomato;
  color: #fff;
  height: 35px;
  margin-right: 5px;
}
.button-icon {
  width: 35px;
  height: 35px;
  padding: 0;
}
.calendar-cont {
  padding: 1rem;
}
.calendar-body {
  position: relative;
}
.calendar-header {
  display: flex;
  width: 100%;
  justify-content: flex-start;
  align-items: center;

  color: gray;
}
.daynames,
.dates {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  width: 100%;
  list-style: none;
  padding: 0;
  margin: 0;
}
.dates {
  border-left: 1px solid;
  border-top: 1px solid;
  border-color: #e0e0e0;
}
.dates li {
  padding: 1.5rem 0.5rem;
  text-align: center;
  border-right: 1px solid;
  border-bottom: 1px solid;
  border-color: #e0e0e0;
  cursor: pointer;
  list-style: none;
  position: relative;
  font-size: 18px;
  color: gray;
}
.daynames li {
  padding: 0.5rem 0.2rem;
  text-align: center;
  color: gray;
  list-style: none;
}
.dates li.today {
  color: #fff;
  background-color: tomato;
}
.dates li.selected {
  background-color: #ffc316;
  color: #fff;
  border-color: #f3b417;
}
.save {
  margin-top: 2rem;
  float: right;
}
.save button {
  padding: 15px 40px;
  border: none;
  border-radius: 8px;
  background-color: tomato;
  color: white;
}
</style>
