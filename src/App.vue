<template>
  <div id="app">
    <div class="container mt-5">
      <div class="form-group">
        <label> <h3>Selecciona el Indicador</h3></label>
        <select  v-model="indicador" class="form-control form-control-lg" name="indicador">
          <option v-for="ind in indicadoresEconomicos" v-bind:key="ind.codigo">
            {{ind.codigo}}
          </option>
        </select>
      </div>

    <spinner v-show="loading"/>
    <div  v-show="indicador != '' && !loading" >
      <h4>Indicador : {{ datos.nombre }}</h4>
      <h5>Unidad de medida: {{ datos.unidad_medida}}</h5>
    </div>
    <table v-show="indicador != '' && !loading" class="container table mt-2">
      <thead class="thead-dark">
        <tr>
          <th scope="col">Fecha</th>
          <th scope="col">Valor</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="d in datos.serie" :key="d.fecha">
          <th scope="row">{{d.fecha | fecha}}</th>
          <td>{{d.valor}}</td>
      </tr>
      </tbody>
    </table>
</div>
  </div>

</template>

<script>
import getIndicadores from './api';
import Spinner from './components/Spinner.vue';

export default {
  name: 'app',
  data(){
    return{
      datos:[],
      indicadoresEconomicos : [
        { codigo : 'uf'},
        { codigo : 'ivp'},
        { codigo: 'dolar'},
        { codigo: 'dolar_intercambio'},
        { codigo: 'euro'},
        { codigo: 'ipc'},
        { codigo: 'utm'},
        { codigo: 'imacec'},
        { codigo: 'tpm'},
        { codigo: 'libra_cobre'},
        { codigo: 'tasa_desempleo'},
        { codigo: 'bitcoin'}
  ],
  indicador:'',
  loading:false,
  indice: Date.now()
    }
  },
  components: {
    Spinner
  },
  methods:{
    refreshIndicadores(){
      const self = this;
      this.loading = true
      this.datos=[]
      getIndicadores(this.indicador)
        .then(function(indicadores){
          self.loading = false
          self.datos = indicadores
          })
    }
  },
  mounted(){
    this.refreshIndicadores();
  },
  watch:{
    indicador(){
      this.refreshIndicadores();  
    },
    datos(){

    }
  },
  filters: {
    fecha: function (value) {
      let f = new Date(value);
      let fecha = `${f.getDate()}/${f.getMonth() + 1}/${f.getFullYear()}`;
      return fecha
  }
}
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 10  px;
}
</style>
