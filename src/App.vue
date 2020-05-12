<template>
  <div id="app">
    <b-row cols="mb-3">
      <div class="col-12">
        <h1 class="results">
          {{ number }}
        </h1>
      </div>
      <div class="col-6">
        <b-button block @click="randomNumber()" :disabled="pendientes<=0">
          Nuevo Número
        </b-button>
      </div>
      <div class="col-6">
        <b-button block variant="info" @click="reset()" >
          Reiniciar
        </b-button>
      </div>
      <div class="col-6">
        <span class="d-block mt-3">
          Números pendientes
          {{ pendientes }}
        </span>
      </div>
      <div class="col-6">
        <b-form-checkbox class="mt-3" v-model="showLetter">
          Mostrar letra en tablero
        </b-form-checkbox>
      </div>
    </b-row>
    <b-row class="m-0 mt-3">
        <div class="col-1 numbers"
             v-for="(num,index) in numerosSelected"
             :key="index"
             :class="{selected:num.selected}">
          {{ letterBoard(num.valor) }}
        </div>
    </b-row>
  </div>
</template>

<script>
  import Swal from 'sweetalert2/dist/sweetalert2.js'
export default {
  name: 'App',
  components: {
  },
  data(){
    return {
      showLetter:false,
      number:0,
      numbers:[],
      selecteds:[],
      pendientes:0
    }
  },
  computed:{
    numerosSelected(){
      if(this.numbers.length == 0){
        return [];
      }
      let numbers = [];
      this.numbers.forEach( (e) => {
        numbers.push({
          valor:e,
          selected: this.pendientes>0?this.selecteds.includes(e):true
        });
      })
      return numbers;
    }
  },
  created() {
    this.init()
  },
  methods:{
    randomNumber(){
      if(this.pendientes<=1){
        let containner =  document.querySelector(".numbers:not(.selected)");
        if(containner==null) {
          return false;
        }
        number = containner.innerText;
        this.selectNumber(number);
        this.pendientes=0;
        return false;
      }
      let number = Math.random() * (76 - 1) + 1;
      number = Math.floor(number);
      if(this.selecteds.includes(number)){
        this.randomNumber()
      }else{
        this.pendientes--;
        this.selectNumber(number);
      }
    },
    selectNumber(number){
      this.selecteds.push(number);
      this.number = this.letter(number);
    },
    letter(number){
      let letra = "";
      if(number <= 15){
        letra = "B";
      }else if(number <= 30){
        letra = "I";
      }else if(number <= 45){
        letra = "N";
      }else if(number <= 60){
        letra = "G";
      }else{
        letra = "O";
      }
      number = number.toString().replace(/[^\d]/, '');
      return letra+number;
    },
    letterBoard(number){
      if(number < 9){
        number = '0'+number;
      }
      return this.showLetter ? this.letter(number) : number;
    },
    init(){
      this.selecteds = [];
      this.numbers = [];
      this.number = "0";
      for(let i = 1; i<=75; i++){
        this.numbers.push(i);
      }
      this.pendientes = 75;
    },
    reset(){
      Swal.fire({
        title: 'Reiniciar tablero',
        text: "Estas seguro de reiniciar el tablero?",
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Si, reiniciar!',
        cancelButtonText: 'Cancelar'
      }).then((result) => {
        if (result.value) {
          this.init();
        }
      })
    }
  }
}
</script>

<style>
  *{
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
  }
  body{
    background: #f5f5f5 !important;
  }
  .results{
    font-size: 5.5rem;
    background: white;
    width: 220px;
    margin: auto auto 15px auto;
    padding: 8px;
    border-radius: 8px;
  }
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  max-width: 600px;
  margin: 20px auto auto;
}
.selected {
  color:white;
  background: red;
}
.numbers{
  padding:5px 0px !important;
  font-weight: bold;
  text-align: center;
  box-shadow: 0 0 0 1px inset #f5f5f5;
}
  @media (max-width:470px){
    .numbers {
      min-width: 38px;
    }
  }
</style>
