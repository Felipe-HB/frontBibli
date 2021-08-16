<template>
<div>
  <div>
    <h5>Nome</h5>
    <input v-model="form.nome" />
  </div>
  <div>
    <h5>Url</h5>
    <input  v-model="form.url" />
  </div>
  <div>
    <h5>telefone</h5>
    <input v-model="form.telefone" />
  </div>
  <div>

    <button v-if="button" @click="createEditoras" >
      Botão
    </button>
    
    <button v-if="!button" @click="updateEditoras" >
      Atualizar
    </button>
  </div>
  <div>
   <div v-for="(editora, index) in editoras" :key="index">
     <h5>{{editora.nome}}</h5>
     <h5>{{editora.url}}</h5>
     <h5>{{editora.telefone}}</h5>
     <h5>{{editora.id}}</h5>

     <button @click="deleteEditoras(editora.id)" > Excluir </button>
     <button @click="editar(editora)"> Editar </button>
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
           url: "",
           telefone: ""
          },

        button: true,
        idEditora: "",
        editoras:[],
     }
  },
mounted(){
  this.getEditoras();
},
 methods:{
   createEditoras(){
     apiLib
      .createEditoras(this.form)
      .then (({data})=> {
        console.log(data)
        this.form ={
                nome: "",
                url:"",
                telefone: "",
        };
        this.getEditoras();
      })
      .catch((err) => {
        console.log(err)
      })
   },

   getEditoras() {
     apiLib
     .getEditoras()
      .then (({data})=> {
          this.editoras = data

      })
     },

    deleteEditoras(id){
        apiLib
        .deleteEditoras(id)
        .then(()=> {
            this.getEditoras()
        })
    },
    editar(editora){
        this.idEditora = editora.id,
        this.form.nome = editora.nome,
        this.form.url = editora.url,
        this.form.telefone = editora.telefone,

        this.button = false
    },
    updateEditoras(){
        apiLib
        .updateEditoras(this.idEditora, this.form)
        .then(()=> {
                    this.form ={
                nome: "",
                url: "",
                telefone: "",
        };
        this.getEditoras();
        this.button = true
        })

    }
   }
 
}
</script>

