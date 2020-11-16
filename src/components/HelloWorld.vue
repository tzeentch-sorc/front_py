<template>
  <div class="hello">
    <div>
      <h3>Difficulty</h3>
      <button v-on:click="setLevel(1)">level 1</button>

      <button v-on:click="setLevel(1)">level 2</button>
      <button v-on:click="setLevel(1)">level 3</button>
    </div>
    <br/>
    <hr/>
    <h2>Who is this person?</h2>
    <img :src="this.info.image"/><br/><br/>
    <button v-on:click="result(1)">{{this.info.correct}}</button>
    <button v-on:click="result(2)">{{this.info.incorrect}}</button>
    <hr/>
    <br/>
    <button v-on:click="getData">New person</button>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  methods:{
    getData: function(){
      const axios = require('axios');
      axios.get('http://localhost:8080/', {withCredentials:true}).then(
          response => {
            if(Math.random() <= 0.5) {
              this.info.correct = response.data.incorrect
              this.info.incorrect = response.data.correct
              this.info.image = response.data.image
              this.info.swapped = true
            } else {
              this.info = response.data;
              this.info.swapped = false;
            }
          }
      ).catch((error) => {
        alert("You won the game!")
        console.log(error)
      })
    },
    setLevel: function (level){
      const axios = require('axios');
      axios.post('http://localhost:8080/', level,{withCredentials:true})
    },
    result: function (button){
      if(button == 1 && !this.info.swapped){
        alert("Correct")
        this.getData();
      } else
      if(button == 2 && this.info.swapped){
        alert("Correct")
        this.getData();
      } else
      alert("Fail");
    }
  },
  data(){
    return {
      info: {
        image: "",
        correct: "",
        incorrect: "",
        swapped: false
      }
    }
  },
  mounted() {
    this.getData();
  }
}
</script>

<style scoped>
button{
  margin: 5px;
}
</style>
