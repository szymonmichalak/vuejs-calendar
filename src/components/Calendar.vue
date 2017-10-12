<template>
  <div :class="$style.container">
    <div class="calendar-container">
      <header>
        <button v-on:click="prevMonth"><i class="arrow left"></i></button>
        <div class="month">{{currentDate.format('MMMM \'YY')}}</div>
        <button v-on:click="nextMonth"><i class="arrow right"></i></button>
      </header>
      <table class="calendar">
        <thead>
          <tr>
            <td v-for="day in getWeekDays(0)">{{day.format('ddd')}}</td>
          </tr>
        </thead>
        <tr v-for="n in 6">
          <td v-for="day in getWeekDays(n - 1)" v-bind:class="getClassName(day)">{{day.format('D')}}</td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
  import moment from 'moment'

  export default {
    data () {
      return {
        currentDate: moment(),
        weeks: []
      }
    },
    methods: {
      getWeekDays (weekInMonth) {
        const monthStart = this.currentDate.clone().startOf('month')
        const weekStart = monthStart.clone().add(weekInMonth, 'w').startOf('week')

        const week = []
        for (let day = 0; day < 7; day++) {
          week.push(weekStart.clone().add(day, 'day'))
        }
        return week
      },
      prevMonth () {
        // this.$set(this.currentDate, this.currentDate.subtract(1, 'month'))
        this.currentDate = this.currentDate.subtract(1, 'month')
        this.$forceUpdate()
      },
      nextMonth () {
        // this.$set(this.currentDate, this.currentDate.add(1, 'month'))
        this.currentDate = this.currentDate.add(1, 'month')
        this.$forceUpdate()
      },
      getClassName (day) {
        if (day.isSame(this.currentDate, 'day')) {
          return 'current-day'
        }
        if (day.isBefore(this.currentDate, 'month')) {
          return 'prev-month'
        }
        if (day.isAfter(this.currentDate, 'month')) {
          return 'next-month'
        }
        return 'current-month'
      }
    }
  }
</script>

<style module>
  .container {
    left: 50%;
    position: fixed;
    top: 50%;
    transform: translate(-50%, -50%);
  }
</style>

<style>
  body {
    background: #ccc;
    font: 87.5%/1.5em 'Lato', sans-serif;
    margin: 0;
  }

  table {
    border-collapse: collapse;
    border-spacing: 0;
  }

  td {
    padding: 0;
  }

  .calendar-container {
    position: relative;
    width: 450px;
  }

  .calendar-container header {
    border-radius: 1em 1em 0 0;
    background: #e66b6b;
    color: #fff;
    padding: 3em 2em;
    display: flex;
    justify-content: space-between;
  }

  .day {
    font-size: 8em;
    font-weight: 900;
    line-height: 1em;
  }

  .month {
    font-size: 2em;
    line-height: 1em;
  }

  .calendar {
    background: #fff;
    border-radius: 0 0 1em 1em;
    -webkit-box-shadow: 0 2px 1px rgba(0, 0, 0, .2), 0 3px 1px #fff;
    box-shadow: 0 2px 1px rgba(0, 0, 0, .2), 0 3px 1px #fff;
    color: #555;
    padding: 2em;
    display: inline-block;
    box-sizing: border-box;
    width: 100%;
  }

  .calendar thead {
    color: #e66b6b;
    font-weight: 700;
    text-transform: uppercase;
  }

  .calendar td {
    padding: .5em 1em;
    text-align: center;
  }

  .calendar tbody td:hover {
    background: #cacaca;
    color: #fff;
  }

  .prev-month,
  .next-month {
    color: #cacaca;
  }

  .current-day {
    color: #e66b6b;
    font-weight: bold;
  }

  .ring-left,
  .ring-right {
    position: absolute;
    top: 250px;
  }

  .ring-left { left: 2em; }
  .ring-right { right: 2em; }

  .ring-left:before,
  .ring-left:after,
  .ring-right:before,
  .ring-right:after {
    background: #fff;
    border-radius: 4px;
    -webkit-box-shadow: 0 3px 1px rgba(0, 0, 0, .3), 0 -1px 1px rgba(0, 0, 0, .2);
    box-shadow: 0 3px 1px rgba(0, 0, 0, .3), 0 -1px 1px rgba(0, 0, 0, .2);
    content: "";
    display: inline-block;
    margin: 8px;
    height: 32px;
    width: 8px;
  }

  .arrow {
    border: solid lightgray;
    border-width: 0 5px 5px 0;
    display: inline-block;
    padding: 5px;
  }

  .right {
    transform: rotate(-45deg);
    -webkit-transform: rotate(-45deg);
  }

  .left {
    transform: rotate(135deg);
    -webkit-transform: rotate(135deg);
  }

  /* reset button */
  button {
    background: none;
    border: 0;
    color: inherit;
    /* cursor: default; */
    font: inherit;
    line-height: normal;
    overflow: visible;
    padding: 0;
    -webkit-appearance: button; /* for input */
    -webkit-user-select: none; /* for button */
    -moz-user-select: none;
    -ms-user-select: none;
  }
  input::-moz-focus-inner,
  button::-moz-focus-inner {
    border: 0;
    padding: 0;
  }
</style>
