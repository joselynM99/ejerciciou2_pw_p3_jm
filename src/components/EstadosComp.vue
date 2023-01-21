<template>
  <h1>Estados</h1>
  <button v-on:click="consumirAPINombre">Mostrar estados</button>
  <div id="div2">
    <div id="div1" v-for="item1 in estados1" :key="item1">{{ item1 }}</div>
    <div id="div1" v-for="item2 in estados2" :key="item2">{{ item2 }}</div>
    <div id="div1" v-for="item3 in estados3" :key="item3">{{ item3 }}</div>
  </div>

  <input v-model="message" placeholder="Codigo de Estado" />
  

<div>
  <label for="">Casos positivos: positive</label>
  <input v-bind:value="positive" />

  <label for="">Total de resultados de pruebas:</label>
  <input value="{{this.totalTestResults}}" />

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
};
</script>

<style>
#div1 {
  display: flex;
  margin: 5px;
}

#div2 {
  display: grid;
}
</style>