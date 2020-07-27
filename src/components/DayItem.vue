<template>


<div class="row">
    <div class="input-container">
        <input 
            class = "field"
            v-bind:class= "{ field_past: checkPast(), 'field_err': !getTimeValid()}"
            v-model.lazy="task.time" 
        >
        <input class="field" v-model="task.task">
    </div>
    <div class="delete-btn" v-on:click="$emit('delTask', index)"></div>

</div>



</template>

<script>


export default {
  props: ['task','index','dayId'],

  data: function() {
    return {

    }
  },

  computed: {


  },



  methods: {
    getTimeValid() {
      const str = this.task.time;
      var pattern = new RegExp("(^[0-1]?[0-9]|^2[0-3]):[0-5][0-9]$");
      return !!pattern.test(str);
    },

    checkPast() {
      const now = new Date();
      let nowDay = now.getDay();
      let realDay = this.dayId;
      let time = this.task.time;
      if (time.length == 4) {time = '0'+time}

      if (realDay === 0) {realDay = 7}
      if (nowDay === 0) {nowDay = 7}
      
      if (nowDay > realDay) {return true}
      if (nowDay < realDay) {return false}
      if ((nowDay === realDay) & (now.toLocaleTimeString() >= time)) {return true};

      return false;
    },

  }


}

</script>

<style>

.row {
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-bottom: 20px;
}


.delete-btn {
    width: 23px;
    height: 23px;
    background-image: url(https://image.flaticon.com/icons/svg/1828/1828843.svg);
    background-size: contain;
    background-color: red;
    cursor: pointer;
    border-radius: 50%;
}

.delete-btn:hover {
    box-shadow: 0px 0px 5px 1px rgba(0,0,0,0.75);
}

.input-container {
    flex: 4;
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #81C784;
    box-shadow: 0px 0px 5px 1px rgba(0,0,0,0.75);
    border-radius: 5px;
    padding: 10px;
    margin-right: 20px;
}

.field {
    flex: 5;
    width: 100%;
    min-width: 150px;
    margin: 0px;
    font-family: 'Exo 2', sans-serif;
    font-size: 20px;
    line-height: 30px;
    height: 35px;
    border-radius: 5px;
    text-align: center;
    background-color: white;
    white-space: nowrap;
    outline: none;
    border: none;
    overflow: hidden;
}

.field_past {
    background-color: #FFCDD2;
}

.field:first-child {
    min-width: 65px;
    flex: 1;
    margin-right: 15px;
}

.field_err {
    box-shadow: 0px 0px 2px 3px red;
}


.field br {
    display:none;
}

.field * {
    display:inline;
    white-space:nowrap;
}

</style>