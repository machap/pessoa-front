<template>
  <div id="app">

    <nav>
      <div class="nav-wrapper blue darken-1">
        <a href="#" class="brand-logo center">Pessoas Front</a>
      </div>
    </nav>

    <div class="container">

      <form @submit.prevent="salvar">

          <label>Nome</label>
          <input type="text" placeholder="Nome" v-model="pessoa.nome">
          <label>Idade</label>
          <input type="number" placeholder="Idade" v-model="pessoa.idade">
          <label>Sexo</label>
          <input type="text" placeholder="Sexo" v-model="pessoa.sexo">

          <button class="waves-effect waves-light btn-small">Salvar<i class="material-icons left">save</i></button>

      </form>

      <table>

        <thead>

          <tr>
            <th>NOME</th>
            <th>IDADE</th>
            <th>SEXO</th>
            <th>OPÇÕES</th>
          </tr>

        </thead>

        <tbody>

          <tr v-for="pessoa of pessoas" :key="pessoa.id" >

            <td>{{ pessoa.nome }}</td>
            <td>{{ pessoa.idade }}</td>
            <td>{{ pessoa.sexo }}</td>
            <td>
              <button class="waves-effect btn-small blue darken-1"><i class="material-icons">create</i></button>
              <button class="waves-effect btn-small red darken-1"><i class="material-icons">delete_sweep</i></button>
            </td>

          </tr>

        </tbody>
      
      </table>

    </div>

  </div>
</template>

<script>
import Pessoa from "./services/pessoas";

export default {
  data() {
    return {
      pessoa: {
        nome: "",
        idade: "",
        sexo: ""
      },
      pessoas: []
    };
  },

  mounted() {
    this.listar();
  },

  methods: {
    listar() {
      Pessoa.listar().then(resposta => {
        this.pessoas = resposta.data;
      });
    },

    salvar() {
      this.pessoa = {};
      Pessoa.salvar(this.pessoa).then(resposta => {
        alert("Salvo com sucesso!");
        this.listar();
      });
    }
  }
};
</script>

<style>
</style>
