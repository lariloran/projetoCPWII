<template>
  <div class="container mt-2">
    <div v-for="(vest, index) in vests" :key="index">
      <b-card :title="vest.name" class="mb-2">
        <b-card-text>({{vest.initial}})</b-card-text>
        <b-card-text>{{vest.description}}</b-card-text>

        <b-button variant="outline-secondary" class="mr-2" @click="edit(index)">Editar</b-button>
        <b-button variant="outline-danger" class="mr-2"  @click="remove(vest,index)">Excluir</b-button>
      </b-card>
    </div>

    <b-modal ref="modalRemove" hide-footer title="Exclusão de tarefa">
      <div class="d-block text-center">
        Deseja realmente excluir {{vestSelected.subject}} ?
      </div>
      <div class="mt-3 d-flex justify-content-end" >
        
        <b-button variant="outline-secondary" class="mr-2" @click="hideModal">Cancelar</b-button>
        <b-button variant="outline-danger" class="mr-2"  @click="confirmRemoveVest">Excluir</b-button>
     
      </div>
    </b-modal>
  </div>
</template>



<script>
export default {
  name: "List",

  data() {
    return {
      vests: [],
      vestSelected : []
    }
  },
  created() {
    this.vests = (localStorage.getItem("vests")) ? JSON.parse(localStorage.getItem("vests")) : [];

  },

  methods:{
    edit(index){
      this.$router.push({name: "form", params: {index}});
    },
    remove(vest, index){
      this.vestSelected = vest;
      this.vestSelected.index = index;
      this.$refs.modalRemove.show();
    },
    hideModal(){
      this.$refs.modalRemove.hide();
    },
    confirmRemoveVest(){
      this.vests.splice(this.vestSelected.index, 1);
      localStorage.setItem("vests", JSON.stringify(this.vests));
      this.hideModal();
    }
  }

}
</script>