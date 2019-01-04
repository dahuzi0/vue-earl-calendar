<template>
  <div id="app">
    <vue-earl-calendar
      :events="demoEvents"
      :defaultSelectedDay="defaultSelectedDay"
      @day-changed="handleDayChanged"
      @month-changed="handleMonthChanged">
    </vue-earl-calendar>
  </div>
</template>

<script>
let today = new Date()
export default {
  name: 'app',
  data () {
    return {
      demoEvents: [{
        date: `${today.getFullYear()}/${today.getMonth() + 1}/${today.getDate()}`,
        title: 'Title-1',
        desc: 'longlonglong description'
      }, {
        date: `${today.getFullYear()}/${today.getMonth() + 1}/${today.getDate()}`,
        title: 'Title-1',
        desc: 'longlonglong description'
      }, {
        date: `${today.getFullYear()}/${today.getMonth() + 1}/${today.getDate()}`,
        title: 'Title-1',
        desc: 'longlonglong description'
      }, {
        date: `${today.getFullYear()}/${today.getMonth() + 1}/${today.getDate()}`,
        title: 'Title-1',
        desc: 'longlonglong description'
      }, {
        date: `${today.getFullYear()}/${today.getMonth() + 1}/${today.getDate()}`,
        title: 'Title-1',
        desc: 'longlonglong description'
      }, {
        date: `${today.getFullYear()}/${today.getMonth() + 1}/${today.getDate()}`,
        title: 'Title-1',
        desc: 'longlonglong description'
      }, {
        date: `${today.getFullYear()}/${today.getMonth() + 1}/15`,
        title: 'Title-1',
        desc: 'longlonglong description'
      }, {
        date: `${today.getFullYear()}/${today.getMonth() + 1}/24`,
        title: 'Title-2'
      }, {
        date: `${today.getFullYear()}/${today.getMonth() === 11 ? 1 : today.getMonth() + 2}/06`,
        title: 'Title-3',
        desc: 'description'
      }],
      defaultSelectedDay: {
        date: '2019/1/24',
        events: this.events || [] // default show all event
      }
    }
  },
  methods: {
    handleMonthChanged (month) {
      console.log(month)
    },
    handleDayChanged (day) {
      console.log(day)
    }
  }
}
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    margin: 0 auto;
    width: 241px;
    background: rgba(255,255,255,1);
    border-radius: 10px;
    box-shadow: 0 6px 5px rgba(0,0,0,.1);
    padding-left: 10px;
    padding-right: 10px;
  }

  h1, h2, h3 {
    font-weight: normal;
    margin: 0;
    padding: 0;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }
  .t-center{
    text-align: center;
    margin: 20px;
  }
</style>
