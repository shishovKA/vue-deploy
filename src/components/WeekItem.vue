<template>
  <p 
    v-on:click="$emit('switchDay', tekDayId)"
    class="dayblock"
    v-bind:class= "{dayblock_current: isCurrent, 'dayblock_dragover': over}"
    v-on:dragover.prevent = 'over = true'
    v-on:dragleave = 'over = false'
    v-on:drop.prevent = "copyTasks"
  > 
    {{title}} 
  </p>
</template>

<script>


export default {
  props: ['title','curDayId','tekDayId'],

  data() { 
    return { 
      over: '',
      }
  },

  computed: {
    isCurrent: function () {
      return this.curDayId == this.tekDayId;
    },

  },

  methods: {
    copyTasks: function() {
     this.over = false;
      this.$emit('copyTasks', this.tekDayId)
    },

  }

}

</script>

<style>

  .dayblock {
      cursor: pointer;
      min-width: 50px;
      margin: 0px;
      margin-bottom: 10px;
      border-radius: 10px;
      background-color: #E8F5E9;
      font-family: 'Exo 2', sans-serif;
      font-size: 20px;
      line-height: 30px;
      box-shadow: 2px 2px 2px 0px rgba(0,0,0,0.75);
      text-align: center;
      position: relative;
  }

  .dayblock_current {
      background-color: #EF9A9A;
  }

  .dayblock_dragover {
      box-shadow: 0px 0px 5px 5px white;
  }
  

  .dayblock:last-child {
      margin: 0px;
  }

  .dayblock:hover {
    top: -2px;
    right: -1px;
  }

</style>