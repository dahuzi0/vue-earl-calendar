<template>
  <div class="__vev_calendar-wrapper">
    <cal-panel
      :events="events"
      :calendar="calendarOptions"
      :selectedDay='selectedDayEvents.date'
      @cur-day-changed="handleChangeCurDay"
      @month-changed="handleMonthChanged">
    </cal-panel>
  </div>
</template>
<script>
import { isEqualDateStr } from '../js/tools.js'

import calPanel from './cal-panel.vue'

const inBrowser = typeof window !== 'undefined'
export default {
  name: 'vue-earl-calendar',
  components: {
    'cal-panel': calPanel
  },
  data () {
    return {
      defaultSelectedDayValue: {
        date: 'all',
        events: this.events || [] // default show all event
      },
      selectedDayEvents: this.defaultSelectedDay ? this.defaultSelectedDay : Object.assign({}, this.defaultSelectedDayValue)
    }
  },
  props: {
    title: String,
    events: {
      type: Array,
      required: true,
      validator (events) {
        let validate = true
        events.forEach((event, index) => {
          if (!event.date) {
            console.error('Vue-Event-Calendar-Error:' + 'Prop events Wrong at index ' + index)
            validate = false
          }
        })
        return validate
      }
    },
    defaultSelectedDay: {
      type: Object,
      required: false
    }
  },
  computed: {
    calendarOptions () {
      let dateObj = new Date()
      if (inBrowser) {
        return window.VueCalendarBarEventBus.CALENDAR_EVENTS_DATA
      } else {
        return {
          options: {
            locale: 'en', // zh
            color: '#827ffe'
          },
          params: {
            curYear: dateObj.getFullYear(),
            curMonth: dateObj.getMonth(),
            curDate: dateObj.getDate(),
            curEventsDate: 'all'
          }
        }
      }
    },
    calendarParams () {
      let dateObj = new Date()
      if (inBrowser) {
        return window.VueCalendarBarEventBus.CALENDAR_EVENTS_DATA.params
      } else {
        return {
          curYear: dateObj.getFullYear(),
          curMonth: dateObj.getMonth(),
          curDate: dateObj.getDate(),
          curEventsDate: 'all'
        }
      }
    }
  },
  created () {
    if (this.calendarParams.curEventsDate !== 'all') {
      this.handleChangeCurDay(this.calendarParams.curEventsDate)
    }
  },
  methods: {
    handleChangeCurDay (date) {
      let events = this.events.filter(function (event) {
        return isEqualDateStr(event.date, date)
      })
      if (events.length > 0) {
        this.selectedDayEvents = {
          date: date,
          events: events
        }
      }
      this.$emit('day-changed', {
        date: date,
        events: events
      })
    },
    handleMonthChanged (yearMonth) {
      this.$emit('month-changed', yearMonth)
    }
  },
  watch: {
    calendarParams () {
      if (this.calendarParams.curEventsDate !== 'all') {
        let events = this.events.filter(event => {
          return isEqualDateStr(event.date, this.calendarParams.curEventsDate)
        })
        this.selectedDayEvents = {
          date: this.calendarParams.curEventsDate,
          events
        }
      } else {
        this.selectedDayEvents = {
          date: 'all',
          events: this.events
        }
      }
    }
  }
}
</script>
<style lang="less">
@base-blue: #827FFE;
@white: #ffffff;
@gray: #e0e0e0;
@gray-dark: #b1b1b1;
@large-padding: 15px;
@small-padding: 10px;

@icon-border-size: 1.5px;
@media screen and (min-width: 768px) {
  .__vev_calendar-wrapper{
    max-width: 1200px;
    margin: 0 auto;
    .cal-wrapper{
      /*width: 50%;*/
      /*padding: 100px 50px;*/
      background-color: #fff;
      border-radius: 10px;
      .date-num{
        line-height: 37px;
      }
    }
    .events-wrapper{
      width: 50%;
      background-color: @base-blue;
      color: @white;
      padding: 40px 45px;
      position: absolute;
      left: 50%;
      top: 0;
      bottom: 0;
    }
  }
}
@media screen and (max-width: 767px) {
  .__vev_calendar-wrapper{
    .cal-wrapper{
      /*width: 100%;*/
      /*padding: 10px 5px;*/
      background-color: #fff;
      border-radius: 10px;
      .date-num{
        line-height: 37px;
      }
    }
    .events-wrapper{
      width: 100%;
      margin-top: 10px;
      padding: 10px;
    }
  }
}
.__vev_calendar-wrapper{
  position: relative;
  overflow: hidden;
  width: 100%;
  *{
    box-sizing: border-box;
  }
  ::-webkit-scrollbar{
    width: 8px;
    height: 8px;
  }
  ::-webkit-scrollbar-track {
    box-shadow: inset 0 0 2px rgba(0,0,0,.2);
    border-radius: 5px;
  }
  ::-webkit-scrollbar-thumb {
    border-radius: 5px;
    background: rgba(0,0,0,.2);
  }
  .cal-wrapper{
    .cal-header{
      position: relative;
      width: 100%;
      background-color: @white;
      font-weight: 500;
      overflow: hidden;
      padding-top: 10px;
      padding-bottom: 10px;
      border-top-left-radius: 10px;
      border-top-right-radius: 10px;
      &>div{
        float: left;
        line-height: 20px;
        padding: @large-padding;
      }
      .title{
        width: 60%;
        text-align: center;
        color: #4b4bfd;
        font-size: 14px;
        font-weight: 900;
      }
      .l{
        text-align: center;
        text-align: -webkit-center;
        width: 20%;
        cursor: pointer;
        user-select: none;
        -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
      }
      .r{
        text-align: center;
        text-align: -webkit-center;
        width: 20%;
        cursor: pointer;
        user-select: none;
        -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
      }
    }
    .cal-body{
      width: 100%;
      .weeks{
        width: 100%;
        overflow: hidden;
        text-align: center;
        font-size: 12px;
        font-weight: 900;
        color: #666;
        .item{
          line-height: 30px;
          float: left;
          width: 14.285%;
        }
      }
      .dates{
        width: 100%;
        overflow: hidden;
        text-align: center;
        color: #a8a8a8;
        font-weight: 900;
        .item{
          position: relative;
          float: left;
          display: block;
          width: 14.285%;
          cursor: default;
          -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
          .date-num{
            font-size: 12px;
            position: relative;
            z-index: 3;
            color: #303030;
          }
          &.event{
            cursor: pointer;
          }
          &.selected-day{
            .is-event{
              background-color: @base-blue;
            }
          }
          .is-event{
            content: '';
            // border: 1px solid @base-blue;
            background-color: #fff;
            border-radius: 50%;
            width: 22px;
            height: 22px;
            position: absolute;
            left: 50%;
            top: 50%;
            z-index: 1;
            margin-left: -11px;
            margin-top: -11px;
          }
          .is-today{
            content: '';
            background-color: @base-blue;
            border-radius: 50%;
            opacity: .8;
            width: 12px;
            height: 4px;
            position: absolute;
            left: 50%;
            top: 50%;
            z-index: 2;
            margin-left: -5.5px;
            margin-top: 8px;
          }
        }
      }
    }
  }
  .events-wrapper{
    border-radius: 10px;
    .cal-events{
      height: 95%;
      overflow-y: auto;
      padding: 0 5px;
      margin: 15px 0;
    }
    .date{
      max-width: 60%;
      min-width: 200px;
      text-align: center;
      color: @white;
      background-color: rgba(0, 0, 0, 0.2);
      border-radius: 20px;
      margin: 0 auto;
      font-size: 22px;
    }
    .event-item{
      padding: 5px 20px;
      margin-top: 15px;
      box-shadow: 0 3px 11px 2px rgba(0,0,0,.1);
      background-color: #fff;
      border-radius: 5px;
      color: #323232;
      position: relative;
      &:first-child{
        margin-top: 0;
      }
      .title{
        height: 40px;
        line-height: 40px;
        color: #4b4bfd;
        font-size: 16px;
        border-bottom: 1px solid #f2f2f2;
      }
      .time{
        position: absolute;
        right: 30px;
        top: 17px;
        color: #9b9b9b;
        font-size: 14px;
      }
      .desc{
        color: #9b9b9b;
        font-size: 14px;
        padding: 7px 0;
      }
    }
  }
  .button-left-bg {
    width: 20px;
    height: 20px;
    position: relative;
    background-color: #4b4bfd;
    margin-left: -7px;
    border-radius: 50%;
  }
  .button-right-bg {
    width: 20px;
    height: 20px;
    position: relative;
    background-color: #4b4bfd;
    margin-left: 6px;
    border-radius: 50%;
  }
  .arrow-left.icon {
    color: #000;
    position: absolute;
    left: 6%;
    margin-top: 10px;
  }
  .arrow-left.icon:before {
    content: '';
    position: absolute;
    left: 7px;
    top: -3px;
    width: 5px;
    height: 5px;
    color: #fff;
    border-top: solid @icon-border-size currentColor;
    border-right: solid @icon-border-size currentColor;
    -webkit-transform: rotate(-135deg);
    transform: rotate(-135deg);
  }
  .arrow-right.icon {
    color: #000;
    position: absolute;
    right: 6%;
    margin-top: 10px;
  }
  .arrow-right.icon:before {
    content: '';
    position: absolute;
    right: 7px;
    top: -3px;
    width: 5px;
    height: 5px;
    color: #fff;
    border-top: solid @icon-border-size currentColor;
    border-right: solid @icon-border-size currentColor;
    -webkit-transform: rotate(45deg);
    transform: rotate(45deg);
  }
  h3, p{
    margin: 0;
    padding: 0;
  }
}

</style>
