<template>
  <div id="app">
    <img id="app_logo" alt="Vue logo" src="./assets/cyberbune.png">

    <div v-for="(data, index) in allData" v-bind:key="data">
      <Steps
        v-show="currentStep == index"
        v-bind:allData="data"
        v-bind:stepId="index"
        v-bind:confirmTotalPrice="confirmTotalPrice" />
    </div>

    <div class="step_list steps">
      <ul>
        <li v-for="(stepName, index) in allData" v-bind:key="stepName" v-show="currentStep < index">
          [{{stepName.title}}]
        </li>
      </ul>
    </div>

    <div class="price_result steps">
      <p>Итого к оплате</p>
      <p>{{current_status.total_price | format}} ₽</p>
    </div>

    <div v-show="currentStep >= allData.length">
      <p>Спасибо за совершенный заказ!</p>
    </div>


  </div>
</template>

<script>
import Steps from './components/Steps.vue'
import axios from 'axios'
import defaultData from './data.json'

export default {
  name: 'App',
  data: function () {
    return {
      allData: defaultData,
      currentStep: 0,
      current_status: {
        total_price: 0,
        stepsPrices: [],
      }
    };
  },
  mounted() {
    axios.get("https://www.sknt.ru/job/frontend/vue/data.json")
      .then(resp => {
        console.log("From server", resp)
        this.allData = resp;
      })
      .catch(err => { console.log(err); });
  },
  components: {
    Steps
  },
  methods: {
    confirmTotalPrice: function (price, stepId) {
      this.current_status.stepsPrices[stepId] = price;
      this.current_status.total_price = this.current_status.stepsPrices.reduce((sum, cur) => { return sum + cur; });

      this.currentStep++;
      
      console.log(this.current_status);
    }
  },
  filters: {
    format: val => `${val}`.replace(/(\d)(?=(\d{3})+([^\d]|$))/g, '$1 '),
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
#app_logo {
  width: 260px;
}

.step_list li {
  width: fit-content;
  list-style-type: none;
  color: #ccc;
  margin-right: 5px;
}
.step_list ul {
  display: flex;
  padding: 0px;
  margin: 0px;
}
.price_result {
  background: #2FCB5A;
  color: white;
  overflow: hidden;
  padding: 0px !important;
  border-radius: 5px;
  margin-top: 10px !important;
}
.price_result p {
  font-size: 22px;
    width: 47%;
    float: left;
    margin: 0px;
    padding: 10px;
}
.price_result p:last-child {
  text-align: right;
}

form div {
    text-align: center;
    color: #00000047;
    /* height: 30px; */
    border: 1px solid #00000047;
    padding: 10px;
    margin: 0 0 10px 0;
    border-radius: 5px;
}
form button { /* unchosen */
    text-align: center;
    color: white;
    background: #212936c9;
    border: 1px solid #00000047;
    padding: 10px;
    margin: 0 0 10px 0;
    border-radius: 5px;
    
    width: 100%;
}
form button:hover { cursor:pointer; }


.steps {
    width: 770px;
    /* height: 100px; */
    /* border: 1px solid black; */
    margin: auto;
    padding: 10px;
    text-align: left;
}
.title {
    font-weight: bold;
    font-size: 15px;
}
.variant {
    max-width: 100%;
    /* border: 1px solid black; */
    min-height: 190px;
    border-radius: 5px;
    border-left: 5px solid #2FCB5A;
    background: rgba(0, 0, 0, 0.041);
    padding: 10px;
    margin-bottom: 10px;
    overflow: hidden;
}
.variant p {
    margin: 0px;
    font-weight: bold;
    font-size: 16px;
}
.variant_description {margin: 10px 0 0 0;}
.variant_description p {
    font-weight: normal;
    line-height: 22px;
}
.var_sides {
    float: left;
}
.left_side {
    width: 70%;
}
.right_side {
    width: 30%;
}
.right_side p {
    text-align: right;
}

.options {
    max-width: 100%;
    border-left: 1px solid #00000047;
    /* height: 130px; */
    padding: 10px;
    min-height: 150px;
}

.options select {
    width: 100%;
    padding: 10px;
    margin: 5px 0 10px 0;
    border-radius: 5px;
}
.options span {
    font-size: 12px;
}
</style>
