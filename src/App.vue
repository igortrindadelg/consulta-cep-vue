<template>
  <div id="container">
    <h1>Consultar CEP</h1>

    <div class="input">
      <input
        v-model="cep"
        type="text"
        name="cep"
        placeholder="DIGITE AQUI O CEP"
        id="cep"
      />

      <button @click="consultar()">Consultar</button>
    </div>

    <div class="resultado">
      <b>{{ resultado }}</b>
      <p>{{ logradouro }} {{ bairro }}</p>
      <p>{{ localidade }} {{ uf }}</p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import axios from 'axios'

export default defineComponent({
  data() {
    return {
      cep: '',
      resultado: '',
      logradouro: '',
      bairro: '',
      localidade: '',
      uf: ''
    }
  },

  computed: {
    cepFormatado(): string {
      const cepFormatado = this.cep.replace('-', '').replace(' ', '').trim()
      return cepFormatado
    }
  },

  methods: {
    async consultar() {
      const response = await axios
        .get('http://viacep.com.br/ws/' + this.cepFormatado + '/json/')
        .then(function (response) {
          return response.data
        })
        .catch(function (error) {
          console.log(error)
          alert('[ERRO] CEP inválido, tente novamente.')
          window.location.reload()
        })

      this.resultado = ''
      this.resultado = `Esse é o resultado para o cep ${this.cepFormatado}: `
      this.logradouro = ` ${response.logradouro},`
      this.bairro = response.bairro
      this.localidade = ` ${response.localidade} -`
      this.uf = response.uf
    }
  }
})
</script>

<style>
* {
  margin: 0;
  overflow-x: hidden;
}

#app {
  margin: none;
  background-color: rgb(172, 217, 224);
  height: 100vh;
  font-family: sans-serif;
  text-align: center;
  color: #000000;
  display: grid;
  align-content: center;
  justify-items: center;
  letter-spacing: 0.1rem;
}

h1 {
  font-size: 20pt;
  margin-bottom: 5vh;
}

.input {
  display: grid;
  grid-gap: 0.2rem;
  align-content: center;
  justify-items: center;
  transition-duration: 0.4s;
}

.input input {
  background-color: rgb(255, 255, 255);
  text-align: center;
  font-size: 12pt;
  border: none;
  width: 50vw;
  height: 8vh;
  border-radius: 5px;
  letter-spacing: 0.1rem;
  border: 2px solid #616161;
  transition-duration: 0.4s;
  color: rgb(0, 0, 0);
}

.input button {
  cursor: pointer;
  border-radius: 5px;
  width: 50vw;
  height: 8vh;
  background-color: rgb(255, 255, 255);
  border: 2px solid #000000;
  color: rgb(0, 0, 0);
  letter-spacing: 0.1rem;
  font-size: 16pt;
  transition-duration: 0.4s;
}
.input button:hover {
  background-color: rgb(5, 65, 73);
  color: white;
}

.resultado {
  margin-top: 5vh;
  display: grid;
  grid-gap: 0.5rem;
}
</style>
