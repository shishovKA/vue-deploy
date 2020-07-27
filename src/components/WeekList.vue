<template>
  
<div class="window_week">
  <WeekItem 
    v-for="day of workData"
    v-bind:title="day.short"
    v-bind:curDayId="curDayId"
    v-bind:tekDayId="day.id"
    v-bind:key="day.id"
    v-on:switchDay="$emit('switchDay', day.id)"
    v-on:copyTasks="$emit('copyTasks', day.id)"
  />
</div>

</template>

<script>

import WeekItem from './WeekItem'

export default {
  name: 'WeekList',
  props: ['workData','curDayId'],
  components: {
    WeekItem,
  }

}
</script>

<style>

.window_week {
    flex: 1 1 0;
    max-width: 80px;
    background-color: #A5D6A7;
    box-shadow: 5px 5px 10px 0px rgba(0,0,0,0.75);
    border-radius: 10px;
    border: solid 5px #4CAF50;
    padding: 10px;
    align-self: flex-start;
    display: flex;
    flex-direction: column;
    align-items: center;
}

</style>