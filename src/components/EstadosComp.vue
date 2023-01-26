<template>
  <h1>Estados</h1>
  <div id="div2">
    <h5 v-for="item1 in estados1" :key="item1">{{ item1 }}</h5>
    <h5 v-for="item2 in estados2" :key="item2">{{ item2 }}</h5>
    <h5 v-for="item3 in estados3" :key="item3">{{ item3 }}</h5>
  </div>

  <input
    v-on:keyup.enter="consumirEstados(siglasEstado)"
    v-model="siglasEstado"
    placeholder="Codigo de Estado"
  />

 
  <div v-if="positive" class="container">
    <label for="">Casos positivos: positive</label>
    <input v-model="positive" readonly/>

    <label for="">Total de resultados de pruebas:</label>
    <input v-model="totalTestResults" readonly/>

    <label for="">Total de hospitalizaciones:</label>
    <input v-model="hospitalizedCurrently" readonly/>

    <label for="">Total de defunciones:</label>
    <input v-model="death" readonly/>

    <label for="">Test virales:</label>
    <input v-model="totalTestsViral" readonly/>

    <label for="">Tasa de muerte:</label>
    <input v-model="deathIncrease" readonly/>
  </div>
</template>

<script>
export default {

  data() {
    return {
      estados: [],
      estados1: [],
      estados2: [],
      estados3: [],
      positive: null,
      totalTestResults: null,
      hospitalizedCurrently: null,
      death: null,
      totalTestsViral: null,
      deathIncrease: null,
      siglasEstado: null,
      mostrar: false
    };
  },
  methods: {
    async consumirAPINombre() {
      const api = await fetch(
        "https://api.covidtracking.com/v1/states/current.json"
      ).then((result) => result.json());

      api.forEach((element) => {
        const { state } = element;
        this.estados.push(state);
      });

      this.estados1 = this.estados.slice(0, 20);
      this.estados2 = this.estados.slice(21, 30);
      this.estados3 = this.estados.slice(31, 55);
    },
    async consumirEstados(codigoEstado) {
      codigoEstado = codigoEstado.toLowerCase()
      const {
        positive,
        totalTestResults,
        hospitalizedCurrently,
        death,
        totalTestsViral,
        deathIncrease,
      } = await fetch(
        "https://api.covidtracking.com/v1/states/" +
          codigoEstado +
          "/current.json"
      ).then((result) => result.json());

      this.positive = positive;
      this.totalTestResults = totalTestResults;
      this.hospitalizedCurrently = hospitalizedCurrently;
      this.death = death;
      this.totalTestsViral = totalTestsViral;
      this.deathIncrease = deathIncrease;
      

    },
  },

  mounted() {
    console.log("mounted")
    this.consumirAPINombre()
  },

  beforeCreate(){
    console.log('beforeCreate')
  },

  beforeMount() {
    console.log('beforeMount')
  },

  created(){
    console.log('created')
  },

  beforeUpdate() {
    console.log('create')
  },

  updated() {
    console.log('updated')
  },

  activated() {
    console.log('activated')
  },

  deactivated(){
    console.log('deactivated')
  },

 beforeUnmount() {
  console.log('beforeUnmount')
 },

 onUnmounted(){
  console.log('onUnmounted')
 },
unmounted(){
  console.log('unmounted')
},
errorCaptured() {
  console.log('errorCaptured')
},

renderTracked() {
  console.log('renderTracked')
},
renderTriggered() {
  console.log('renderTriggered')
},

  
};
</script>

<style>
#div2 {
  display: grid;

  grid-template-columns: auto auto auto auto auto auto;
}

.container {
  display: grid;
  
  
}

input {
  width: 200px;
  margin: 10px auto;
 
}
</style>