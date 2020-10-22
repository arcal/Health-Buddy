<template>
  <div class="hello">
    <h1>Ask your Health Buddy!</h1>
    <h1>*Snack Edition*</h1>
    <h1>{{msg}}</h1>
    <h2>Your BMI index is: {{bmi}}</h2>   

    <!-- <p id="ogtime"></p>
    <p id="time"></p> -->


    <form>
  <div>
      <label for="height">Height (m):</label>
      <input type='text' v-model="height"/>
  </div>
  <div>
  <div>

  </div>
      <label for="weight">Weight (kgs):</label>
      <input type='text' v-model="weight"/>
  </div>

  <div>
     <label for="age">Age (yrs):</label>
      <input type='text' v-model="age"/>
  </div>

  <div>
    <label for="pa">Physical Activity Intensity:</label>
    <button @click="sedentary"> Sedentary </button>
    <button @click="lowactive"> Low Active </button>
    <button @click="active"> Active </button>
    <button @click="veryactive"> Very Active </button>
  </div>
  <button v-on:click="checkHealth"> Check Health </button>
    </form>
    <quagga-scanner :onDetected="logIt" :readerSize="readerSize" :readerType="'ean_reader'"></quagga-scanner>
    <quagga-scanner :onDetected="checkHealth" :readerSize="readerSize" :readerType="'ean_reader'"></quagga-scanner>

  </div>
</template>



<<script>
import { QuaggaScanner } from 'vue-quaggajs'
export default {
  name: 'VueBarcodeTest',
  data () {
    return {
      height: 0,
      weight: 0,
      maxCal: 0,
      calories: 0,
      bmi: 0,
      age: 0,
      pa: 0,
      //ctr: 0,
      sumCal: 0,
      //time: 0,
      //ogtime: 0,
      msg:'',
      ediblefood: [],
      readerSize: {
        width: 640,
        height: 480
      },
      detecteds: [],
      food: [{barcode:'0829515300579', name: "veggie straws", type: "unhealthy", calorie:'130'},
      {barcode:'0024100940141', name: "cheez-it", type: "unhealthy", calorie: '140'},
      {barcode: '0850251004018', name: "skinny pop", type: "healthy", calorie: '100'},
      {barcode: '0041190061907', name: "seedless raisins", type: "healthy", calorie: '90'},]
    }
  },
  components: {
    QuaggaScanner
  },
  methods: {
    logIt (data) {
      console.log('detected', data.codeResult.code);
      //this.msg = 'Hey you can eat this food' 
    },

    checkHealth(data){
      this.bmi = this.weight/this.height;
      this.maxCal = .25*(387-7.31*this.age+this.pa*(10.9*this.weight+660.7*this.height));
  
     for (var x in this.food){
        //console.log('in for loop',this.food[x].name);
        if(this.food[x].barcode == data.codeResult.code){           
              this.sumCal = +this.sumCal + +this.food[x].calorie; 
              console.log(this.food[x].name);

           
        } 
        if(this.sumCal/this.maxCal >= 0.75 && this.sumCal/this.maxCal <=1){
          if(this.height == 0 || this.weight == 0 || this.age == 0){
            continue;
          }
          console.log("Warning: You are approaching your daily calorie intake limit");
          
        } else if(this.sumCal>this.maxCal){
          if(this.height == 0 || this.weight == 0 || this.age == 0){
            continue;
          }

          console.log("You have exceeded your calorie intake limit");
          
        }
      }
      
      
      console.log('calories: ',this.sumCal);
      

    },

    sedentary(){
      this.pa = 1;
    },

    lowactive(){
      this.pa = 1.14;
    },

    active(){
      this.pa = 1.27;
    },

    veryactive(){
      this.pa = 1.45;
    },
  }
}
  

  

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
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
</style>
