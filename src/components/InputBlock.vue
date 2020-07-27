<template>
    <!--Область ввода-->
<div class="inputblock-container">
<form v-on:submit.prevent= "addTask" class='inputblock'>   
    <div class="input-container">
        <input name='time' type="text" class="field" placeholder="Время"
            v-model="time" 
            v-bind:class= "{field_err: !isTimeValid}"
            v-on:input= "isTimeValid = getTimeValid()"
            
        >
        <input name='task' type="text" class="field" placeholder="Задание" v-model="task">
    </div>

    <input value="" type="submit" class="inputblock__add" v-on:click.prevent = 'addTask'>

</form>
</div>

</template>

<script>


import { v4 as uuidv4 } from 'uuid';


export default {
  props: [],

  data() { 
    return { 
      task: '',
      time: '',
      isTimeValid: true,
      }
  },

  computed: {


  },

  methods: {
    addTask: function() {
        if (this.getTimeValid()) {
            this.$emit('addTask', {id: uuidv4(), task: this.task, time: this.time});
            this.time = '';
            this.task = '';
            }  
    },

    getTimeValid() {
      const str = this.time;
      var pattern = new RegExp("(^[0-1]?[0-9]|^2[0-3]):[0-5][0-9]$");
      var halfPar = new RegExp("^[0-2]+[0-9]$");

      if (!!halfPar.test(str)) {this.time = this.time+':'}

      this.isTimeValid = !!pattern.test(str);
      return !!pattern.test(str);
    }

  },

}

</script>

<style>

.inputblock-container {
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 20px;
    background-color: #E8F5E9;
    box-sizing: border-box;
    border-radius: 10px;
    box-shadow: 0px 0px 5px 1px rgba(0,0,0,0.75);
}

.inputblock {
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-bottom: 20px;
}

.inputblock:last-child {
    margin-bottom: 0px;
}

.inputblock__add {
    border: none;
    width: 50px;
    height: 50px;
    background-size: contain;
    cursor: pointer;
    background-image: url(https://image.flaticon.com/icons/svg/3143/3143540.svg);
}


</style>