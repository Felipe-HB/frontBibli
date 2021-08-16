<template>
<div>
  <div>
    <h5>Nome</h5>
    <input v-model="form.nome" />
  </div>
  <div>
    <h5>Data de Nascimento</h5>
    <input  v-model="form.data_nasc" />
  </div>
  <div>
    <h5>sexo</h5>
    <input v-model="form.sexo" />
  </div>
  <div>
    <h5>Nacionalidade</h5>
    <input v-model="form.nacionalidade" />
  </div>
  <div>

    <button v-if="button" @click="createAutor" >
      Botão
    </button>
    
    <button v-if="!button" @click="updateAutor" >
      Atualizar
    </button>
  </div>
  <div>
   <div v-for="(autor, index) in autores" :key="index">
     <h5>{{autor.nome}}</h5>
     <h5>{{autor.data_nasc}}</h5>
     <h5>{{autor.sexo}}</h5>
     <h5>{{autor.nacionalidade}}</h5>
     <button @click="deleteAutor(autor.id)" > Excluir </button>
     <button @click="editar(autor)"> Editar </button>
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
           nome:"",
           data_nasc: null,
           sexo: "",
           nacionalidade:""
          },

        button: true,
        idAutor: "",
        autores:[],
     }
  },
mounted(){
  this.getAutores();
},
 methods:{
   createAutor(){
     apiLib
      .createAutor(this.form)
      .then (({data})=> {
        console.log(data)
        this.form ={
                nome: "",
                data_nasc: null,
                sexo: "",
                nacionalidade: ""
        };
        this.getAutores();
      })
      .catch((err) => {
        console.log(err)
      })
   },

   getAutores() {
     apiLib
     .getAutores()
      .then (({data})=> {
          this.autores = data

      })
     },

    deleteAutor(id){
        apiLib
        .deleteAutor(id)
        .then(()=> {
            this.getAutores()
        })
    },
    editar(autor){
        this.idAutor = autor.id,
        this.form.nome = autor.nome,
        this.form.data_nasc = autor.data_nasc,
        this.form.sexo = autor.sexo,
        this.form.nacionalidade = autor.nacionalidade

        this.button = false
    },
    updateAutor(){
        apiLib
        .updateAutor(this.idAutor, this.form)
        .then(()=> {
                    this.form ={
                nome: "",
                data_nasc: null,
                sexo: "",
                nacionalidade: ""
        };
        this.getAutores();
        this.button = true
        })

    }
   }

}
</script>

