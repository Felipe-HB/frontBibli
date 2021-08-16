<template>
<div>
  <div>
    <h5>Titulo</h5>
    <input v-model="form.titulo" />
  </div>
  <div>
    <h5>Ano de lançamento</h5>
    <input v-model.number="form.ano_lancamento" />
  </div>
  <div>
    <h5>editora</h5>
    <input v-model="form.editora" />
  </div>
  <div>
    <h5>Genero</h5>
    <input v-model="form.genero" />
  </div>
  <div>
    <h5>Autor</h5>
    <input v-model="form.autor" />
  </div>
  <div>

    <button v-if="button" @click="createLivros" >
      Botão
    </button>
    
    <button v-if="!button" @click="updateLivros" >
      Atualizar
    </button>
  </div>
  <div>
   <div v-for="(livro, index) in livros" :key="index">
     <h5>{{livro.titulo}}</h5>
     <h5>{{livro.ano_lancamento}}</h5>
     <h5>{{livro.editora}}</h5>
     <h5>{{livro.genero}}</h5>
     <h5>{{livro.autor}}</h5>
     <button @click="deleteLivros(livro.id)" > Excluir </button>
     <button @click="editar(livro)"> Editar </button>
   </div>
  </div>
</div>
</template>
<script>
import apiLib from "../../services/config"


export default {

  data(){
 return {
       form:{
           titulo:"",
           ano_lancamento: null,
           editora: "",
           genero:"",
           autor:""
          },

        button: true,
        idLivro: "",
        livros:[],
     }
  },
mounted(){
  this.getLivros();
},
 methods:{
   createLivros(){
     apiLib
      .createLivros(this.form)
      .then (({data})=> {
        console.log(data)
        this.form ={
                titulo: "",
                ano_lancamento: null,
                editora: "",
                genero: "",
                autor:""
        };
        this.getLivros();
      })
      .catch((err) => {
        console.log(err)
      })
   },

   getLivros() {
     apiLib
     .getLivros()
      .then (({data})=> {
          this.livros = data

      })
     },

    deleteLivros(id){
        apiLib
        .deleteLivros(id)
        .then(()=> {
            this.getLivros()
        })
    },
    editar(livro){
        this.idLivro = livro.id,
        this.form.titulo = livro.titulo,
        this.form.ano_lancamento = livro.ano_lancamento,
        this.form.editora = livro.editora,
        this.form.autor = livro.autor
        this.form.genero = livro.genero

        this.button = false
    },
    updateLivros(){
        apiLib
        .updateLivros(this.idLivro, this.form)
        .then(()=> {
                    this.form ={
                titulo: "",
                ano_lancamento: null,
                editora: "",
                autor: "",
                genero:""
        };
        this.getLivros();
        this.button = true
        })

    }
   }
 
}
</script>

