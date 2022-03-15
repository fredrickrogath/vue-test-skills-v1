<template>
<div :class="['test',box1computed]"
@click="selectedBox(1)"
>
  <h1> {{counter}} </h1>
  <h1 v-once> {{counter}} </h1>
</div>
<!-- <input type="text" v-bind="{newName}" @click="changeName()"> -->
<div class="test" :class="['test',box2computed]"
@click="selectedBox(2)"
>
  <button @click="add(5)">add 5</button>
  <button @click="remove(3)">remove 3</button>
</div>

<div class="test" :class="['test',box3computed]"
@click="selectedBox(3)"
>
  <p>full name : {{ fullNameComputed }}</p>
<input type="text" v-model="name">
<input type="text" v-model="lastName">
<br>
<button @click="resetName">reset fullName</button>
</div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'

export default {
  // name: 'App',
  // components: {
  //   HelloWorld
  // }
  data(){
    return {
      counter:0,
      name:'',
      lastName:'',
      fullName:'',
      isBox1:false,
      isBox2:false,
      isBox3:false,
    }
  },
  methods:{
    add(num){
      this.counter += num;
    },
    remove(num){
      this.counter -= num;
    },
    setName(event){
      this.name = event.target.value;
    },
    resetName(){
      console.log('reset name executed');
      this.name = '';
      this.lastName = '';
    },
    selectedBox(num){
      if(num === 1){
        this.isBox1 = !this.isBox1;
      }else if(num === 2){
        this.isBox2 = !this.isBox2;
      }else if(num === 3){
        this.isBox3 = !this.isBox3;
      }
    },
  },
  watch:{
    counter(value){
      if(value > 20){
        this.counter = 0;
      }
    }
    // name(value){
    //   if(value === ''){
    //     this.fullName = '';
    //   }else{
    //     this.fullName = value + ' ' + this.lastName;
    //   }
    // },
    // lastName(value){
    //   if(value === ''){
    //     this.fullName = '';
    //   }else{
    //     this.fullName = this.name + ' ' + value;
    //   }
    // }
  },
  computed:{
    fullNameComputed(){
      console.log('full name executed');
      if(this.name === '' || this.lastName === ''){
        return '';
      }else{
        return this.name + ' ' + this.lastName ;
      }
    },
    box1computed(){
    return { active: this.isBox1 };
    },
    box2computed(){
    return { active: this.isBox2 };
    },
    box3computed(){
    return { active: this.isBox3 };
    }
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
.test{
  border: 3px dotted #2c3e50;
  margin: 7px;
  padding: 5px;
}
.active{
  background-color:rgb(230, 228, 228);
}
</style>
