<template>

  <div class="inner-tt">  
    <transition-group name="task-appear">
      <DayItem class="task-appear-item"
        v-for = "(task, index) of taskList"
        v-bind:task = "task" 
        v-bind:key = 'task.id'
        v-bind:index = 'index'
        v-on:delTask = "$emit('delTask', index)"
        v-bind:dayId = 'dayId'
      />
    </transition-group>
  </div>   
    
    

</template>

<script>

import DayItem from './DayItem'

export default {
  components: {
    DayItem,
  },

  props: ['taskList', 'dayId'],

}

</script>

<style scoped>

.task-appear-item {
  opacity: 1;
  transition: all 0.4s;
}

.task-appear-enter {
  opacity: 0;
  transform: translateY(200px);
  
}

.task-appear-move {
  transition: all 0.4s;
}

.task-appear-leave-active {
  display: none; 
}

.inner-tt {
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    height: 100%;
    overflow-y: scroll;
    padding: 20px;
    box-sizing: border-box;
}


</style>