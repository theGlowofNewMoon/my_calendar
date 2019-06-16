<template>
  <div class="calendar-container">
    <div class="calendar-header">
      <ul>
        <li class="el-icon-arrow-left" @click="getLastMonth()"></li>
        <li class="date-header" @click="getNextMonth()">
          <div>{{year}}</div>
          <div>{{month}}月</div>
        </li>
        <li class="el-icon-arrow-right"></li>
      </ul>
    </div>
    <div class="calendar-content">
      <ul class="week-label">
        <li v-for="item in labelList">{{item}}</li>
      </ul>
      <ul class="date-item">
        <li v-for="item in lastMonthDays">{{item.getDate()}}</li>
        <li v-for="item in thisMonthDays">{{item.getDate()}}</li>
        <li v-for="item in nextMonthDays">{{item.getDate()}}</li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "calendar",
  data() {
    return {
      labelList: ["周一", "周二", "周三", "周四", "周五", "周六", "周日"],
      year: "2017",
      month: "05",
      lastMonthDays: [],
      thisMonthDays: [],
      nextMonthDays: [],
      $now: new Date()
    };
  },
  methods: {
    initData: function() {
      var $now = this.$now;
      var $year = $now.getFullYear();
      this.year = $year;
      var $month = $now.getMonth();
      var currentMonthLength = this.getMonthLength($now);
      var $lastMonth = $month === 1 ? 12 : $month - 1;
      var $nextMonth = $month === 12 ? 1 : $month + 1;
      this.month = $month + 1 < 10 ? "0" + ($month + 1) : $month + 1 + "";
      //   var $week = $now.getDay();
      var $week = new Date(new Date().setDate(1)).getDay();
      var $day = $now.getDate();
      var lastMonthLength = $week ? $week - 1 : 6;
      var nextMonthLength = 35 - currentMonthLength - lastMonthLength;
      this.thisMonthDays = this.getMonthPart($now, currentMonthLength);
      $now.setMonth($month - 1);
      this.lastMonthDays = this.getMonthPart($now, -lastMonthLength);
      $now.setMonth($month + 1);
      this.nextMonthDays = this.getMonthPart($now, nextMonthLength);
    },
    getMonthLength: function(date) {
      var year = date.getFullYear();
      var month = date.getMonth() + 1;
      if ([1, 3, 5, 7, 8, 10, 12].includes(month)) {
        return 31;
      } else if ([4, 6, 9, 11].includes(month)) {
        return 30;
      } else {
        if ((year % 4 === 0 && year % 100 !== 0) || year % 400 === 0) {
          return 29;
        } else {
          return 28;
        }
      }
    },
    getMonthPart: function(date, length) {
      console.log(length);
      var monthLength = this.getMonthLength(date);
      var start = 1,
        end = 1;
      if (monthLength < Math.abs(length) || typeof length !== "number") {
        console.error();
        return;
      }
      if (length < 0) {
        start = monthLength + length + 1;
        end = monthLength;
      } else {
        end = length;
      }
      var list = [];
      for (; start <= end; start++) {
        date.setDate(start);
        list.push(new Date(date));
      }
      console.log(list.map(item => item.getDate()));
      return list;
    },
    getLastMonth: function() {
      this.$now = new Date(this.$now.setMonth(-1));
      this.initData();
    },
    getNextMonth: function() {
      this.$now = new Date(this.$now.setMonth(this.$now.getMonth + 1));
      this.initData();
    }
  },
  created: function() {
      this.$now = new Date();
    this.initData();
  }
};
</script>

<style>
.calendar-container {
}
.calendar-header {
  background-color: skyblue;
  height: 50px;
}
.calendar-header > ul {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.el-icon-arrow-left {
  font-weight: 700;
  font-size: 30px;
  cursor: pointer;
}
.el-icon-arrow-right {
  font-weight: 700;
  font-size: 30px;
  cursor: pointer;
}
.date-header {
  min-width: 200px;
  width: 40%;
  height: 100%;
  text-align: center;
}
.date-header div {
}
.calendar-content {
}
.week-label {
  height: 28px;
  text-align: center;
  background-color: skyblue;
  display: flex;
  align-items: center;
  border-top: 1px solid #f2f2f2;
}
.week-label li {
  flex: 1 1;
}
.week-label li:nth-last-child(1) {
  color: orangered;
}
.week-label li:nth-last-child(2) {
  color: orangered;
}
.date-item {
  display: flex;
  flex-wrap: wrap;
}
.date-item li {
  text-align: center;
  flex: 0 0 calc(0px + (100% - 0px) * 1 / 7);
  height: 100px;
  padding-top: 20px;
  background-color: #f2f2f2;
}
</style>
