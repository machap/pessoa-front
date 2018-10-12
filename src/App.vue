<template>
  <div id="app">

    <nav>
      <div class="nav-wrapper blue darken-1">
        <a href="#" class="brand-logo center">Cadastro de Pessoas</a>
      </div>
    </nav>

    <div class="container">

      <form @submit.prevent="salvar">

          <label>Nome</label>
          <input type="text" placeholder="Nome" required v-model="pessoa.nome">
          <label>Idade</label>
          <input type="number" placeholder="Idade" required v-model="pessoa.idade">
          <label>Sexo</label>
          <input type="text" placeholder="Sexo" required v-model="pessoa.sexo">

          <ul>
            <li v-for="(erro, index) of errors" :key="index">
              campo <b>{{erro.field}}</b> - {{erro.defaultMessage}}
            </li>
          </ul>
      
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
              <button @click="editar(pessoa)" class="waves-effect btn-small blue darken-1"><i class="material-icons">create</i></button>
              <button @click="remover(pessoa)" class="waves-effect btn-small red darken-1"><i class="material-icons">delete_sweep</i></button>
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
  name: "app",
  data() {
    return {
      pessoa: {
        id: "",
        nome: "",
        idade: "",
        sexo: ""
      },
      pessoas: [],
      errors: []
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
      if (this.pessoa.id) {
        Pessoa.atualizar(this.pessoa)
          .then(resposta => {
            this.pessoa = {};
            alert("Atualizado com sucesso!");
            this.listar();
            errors - [];
          })
          .catch(e => {
            this.errors = e.response.data.errors;
          });
      } else {
        Pessoa.salvar(this.pessoa)
          .then(resposta => {
            this.pessoa = {};
            alert("Salvo com sucesso!");
            this.listar();
            errors - [];
          })
          .catch(e => {
            this.errors = e.response.data.errors;
          });
      }
    },

    editar(pessoa) {
      this.pessoa = pessoa;
    },

    remover(pessoa) {
      if (confirm("Deseja excluir o registro ?")) {
        Pessoa.apagar(pessoa)
          .then(resposta => {
            this.listar();
            this.errors = [];
          })
          .catch(e => {
            this.errors = e.response.data.errors;
          });
      }
    }
  }
};

</script>

<style>
</style>
