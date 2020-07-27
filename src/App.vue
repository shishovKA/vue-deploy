<template>
  <div id="app">
    <div class="main-container">

      <div class="window_day">
        <!--кнопка *-->
        <StarBtn />

        <!--Расписание-->
        
        <div class="timetable">
        <transition name="day-change" mode='out-in'>
        <DayList 
          v-bind:key='curDayId'
          v-bind:taskList = 'getCurTable'
          v-bind:dayId = 'curDayId'
          v-on:delTask = "delTask"
        />
        </transition>
        </div>
      
        <!--Область ввода-->
        <InputBlock 
          v-on:addTask = "addTask"
        /> 
      </div>

      <!--Таблица дней недели-->
      <WeekList 
        v-bind:workData="workData"
        v-bind:curDayId="curDayId"
        v-on:switchDay="switchDay"
        v-on:copyTasks="copyTasks"
      />

    </div>
  </div>
</template>

<script>

import StarBtn from '@/components/StarBtn'
import DayList from '@/components/DayList'
import InputBlock from '@/components/InputBlock'
import WeekList from '@/components/WeekList'

import { v4 as uuidv4 } from 'uuid';

export default {
  name: 'App',

  components: {
    StarBtn,
    DayList,
    InputBlock,
    WeekList,
  },
  
  data() { 
    return { 
      startData: [
      {id: 1, name: 'monday', short: 'пн', table: [{id: uuidv4(), time: "13:31", task: "Понедельник"}, {id: uuidv4(), time: "16:00", task: "Скайпкол"}]},
      {id: 2, name: 'tuesday', short: 'вт', table: [{id: uuidv4(), time: "13:32", task: "Вторник"}, {id: uuidv4(), time: "16:00", task: "Скайпкол"}]},
      {id: 3, name: 'wednesday', short: 'ср', table: [{id: uuidv4(), time: "13:33", task: "Среда"}, {id: uuidv4(), time: "16:00", task: "Скайпкол"}]},
      {id: 4, name: 'thursday', short: 'чт', table: [{id: uuidv4(), time: "13:34", task: "Четверг"}, {id: uuidv4(), time: "16:00", task: "Скайпкол"}]},
      {id: 5, name: 'friday', short: 'пт', table: [{id: uuidv4(), time: "13:35", task: "Пятница"}, {id: uuidv4(), time: "16:00", task: "Скайпкол"}]},
      {id: 6, name: 'saturday', short: 'сб', table: [{id: uuidv4(), time: "18:36", task: "Суббота"}, {id: uuidv4(), time: "16:00", task: "Скайпкол"}]}, 
      {id: 0, name: 'sunday', short: 'вс', table: [{id: uuidv4(), time: "13:30", task: "Воскр"}, {id: uuidv4(), time: "16:00", task: "Скайпкол"}]},
      ],

      workData: [],
      curDayId: new Date().getDay(),
      storageName: 'spaVueData',
      }
  },

  created: function () {
    this.loadStartData();
  },

  watch: {
  
    workData: {
      deep: true,
      handler: function() { 
        localStorage.clear();
        localStorage[this.storageName] = JSON.stringify(this.workData);
      }
     },

  },

  computed: {
  
    getCurTable: function () {
      return this.sortTable(this.getDayById(this.curDayId));
    }

  },

  methods: {

    //Загружаем начальный массив, либо из LocalStorage либо из startData
    loadStartData: function() {
      if (localStorage[this.storageName]) {
        this.workData = JSON.parse(localStorage[this.storageName]);
      }
        else {
          this.workData = JSON.parse(JSON.stringify(this.startData));
        }      
    },

    getDayById: function(id) {
      const day = this.workData.find((element) => element.id === id);
      return day;
    },


    switchDay: function(id) {
      this.curDayId = id;
    },

    copyTasks: function(id) {
      const dayToCopy = this.getDayById(this.curDayId);
      const dayToPaste = this.getDayById(id);
      dayToPaste.table = JSON.parse(JSON.stringify(dayToCopy.table));
      this.switchDay(id);
    },


    addTask: function(newTask) {
      const day = this.getDayById(this.curDayId);
      day.table.push(newTask);
    },

    delTask: function(index) {
      const day = this.getDayById(this.curDayId);
      day.table.splice(index, 1);
    },

    ttimeForm: function(time) {
      if (time.length == 4) {
        return '0'+time;
      }
      return time;
    },

    sortTable: function (day) {
      const mas = day.table.sort( (a, b) => {
        if (this.ttimeForm(a.time) < this.ttimeForm(b.time)) {return -1}
        if (this.ttimeForm(a.time) > this.ttimeForm(b.time)) {return 1}
        return 0;
      });
      return mas;
    },

  },

}

</script>

<style>

@import url(./vendor/normalize.css);

.day-change-enter-to, .day-change-leave {
  transition: all 0.5s;
  opacity: 1;
}

.day-change-enter, .day-change-leave-to {
  opacity: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.main-container {
  margin-top: 150px;
  width: 100%;
  max-width: 780px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #FFFFFF;
  margin-right: auto;
  margin-left: auto;
}

.timetable {
    width: 100%;
    margin: 20px 0px;
    height: 550px;
    overflow-x: hidden;
    box-sizing: border-box;
    border-radius: 10px;
    box-shadow: 0px 0px 5px 1px rgba(0,0,0,0.75);
    background-color: #E8F5E9;
}

.window_day {
  flex: 5 5 0;
  margin-right: 20px;
  background-color: #A5D6A7;
  box-shadow: 5px 5px 10px 0px rgba(0,0,0,0.75);
  border-radius: 10px;
  padding: 20px;
  border: solid 5px #4CAF50;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
}

</style>
