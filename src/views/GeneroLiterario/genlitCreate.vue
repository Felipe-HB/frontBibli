<template>
<div>
  <div>
    <h5>Nome</h5>
    <input v-model="form.nome" />
  </div>
  <div>
    <h5>Descrição</h5>
    <input v-model="form.descricao" />
  </div>
  <div>

    <button v-if="button" @click="createGeneros" >
      Botão
    </button>
    
    <button v-if="!button" @click="updateGeneros" >
      Atualizar
    </button>

    <pre>{{idGenero}}</pre>
  </div>
  <div>
   <div v-for="(genero, index) in generos" :key="index">
     <h5>{{genero.nome}}</h5>
     <h5>{{genero.descricao}}</h5>
     <button @click="deleteGeneros(genero.id)" > Excluir </button>
     <button @click="editar(genero)"> Editar </button>
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
           descricao: "",
          },

        button: true,
        idGenero: "",
        generos:[],
     }
  },
mounted(){
  this.getGeneros();
},
 methods:{
   createGeneros(){
     apiLib
      .createGeneros(this.form)
      .then (({data})=> {
        console.log(data)
        this.form ={
                nome: "",
                descricao: ""
        };
        this.getGeneros();
      })
      .catch((err) => {
        console.log(err)
      })
   },

   getGeneros() {
     apiLib
     .getGeneros()
      .then (({data})=> {
          this.generos = data

      })
     },

    deleteGeneros(id){
        apiLib
        .deleteGeneros(id)
        .then(()=> {
            this.getGeneros()
        })
    },
    editar(genero){
        this.idGenero = genero.id,
        this.form.nome = genero.nome,
        this.form.descricao = genero.descricao,

        this.button = false
    },
    updateGeneros(){
        apiLib
        .updateGeneros(this.idGenero, this.form)
        .then(()=> {
                    this.form ={
                nome: "",
                descricao: ""
        };
        this.getGeneros();
        this.button = true
        })

    }
   }
 
}
</script>

