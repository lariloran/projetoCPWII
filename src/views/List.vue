<template>
  <div class="container mt-2">
    <template v-if="isVestsEmpty">
      <div class="empty-data mt-2">
        <img src="../assets/images/empty-data.svg" class="empty-data-image">
        <b-button 
        variant="outline-primary" 
        class="mt-2" 
        size="lg"
        to="/form"
        >Criar vestibular</b-button>
      </div>>
    </template>
<template v-else>
  <div v-for="(vest, index) in vests" :key="index">
      <b-card :title="vest.initial" class="mb-2">
        <b-card-text>[{{vest.name}}]</b-card-text>
        <b-card-text>{{vest.description}}</b-card-text>

        <b-button variant="outline-secondary" class="mr-2" @click="edit(index)">Editar</b-button>
        <b-button variant="outline-danger" class="mr-2"  @click="remove(vest,index)">Excluir</b-button>
      </b-card>
    </div>
</template>

    <b-modal ref="modalRemove" hide-footer title="Exclusão de vestibular">
      <div class="d-block text-center">
        Deseja realmente excluir {{vestSelected.name}} ?
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
  },

  computed:{
    isVestsEmpty(){
      return this.vests.length === 0;
    }
  }

}
</script>

<style>
.empty-data{
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
.empty-data-image{
  width: 300px;
  height: 300px;
}
</style>