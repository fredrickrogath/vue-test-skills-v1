<template>
<header>Goals App</header>
<section>
    <h1>My course goal</h1>
    <input type="text" v-model="enterGoalValue">
    <button @click="addGoal()">Add</button>

    <p v-if="goals.length === 0">No goals has been added yet</p>

    <ul v-else>
        <li v-for="(goal, index) in goals" :key="index" @click="removeGoal(index)"> {{goal}} - {{index}} </li>
    </ul>
</section>
</template>

<script>

export default {
  data(){
    return {
        enterGoalValue:'',
      goals: [],
    }
  },
  methods:{
    addGoal(){
        this.goals.push(this.enterGoalValue);
        this.enterGoalValue = '';
    },
    removeGoal(index){
        this.goals.splice(index ,1);
    }
  },
  watch:{
    
  },
  computed:{
    
  },
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
