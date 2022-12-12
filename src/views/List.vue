<template>
  <div class="container mt-2">

<b-form inline class="mb-2">
  <b-form-input
  v-model="filter.name"
  id="name"
  placeholder="Ex: Vestibular 2023"
  class="mr-2"
  autocomplete="off"
  >
</b-form-input>
  <b-form-select
v-model="filter.status"
:options="optionsList"
class="mr-2">
</b-form-select>
<b-button variant="outline-secondary"
@click="filterVests"
class="mr-2"
><i class="fa fa-search"></i></b-button>

<b-button variant="outline-secondary"
@click="clearFilter"
class="mr-2"
><i class="fa fa-times"></i></b-button>
</b-form>
<template v-if="isLoading">
  <div class="loading-spin">
    <b-spinner style="width:5rem; height: 5rem;"></b-spinner>
  </div>
</template>

    <template v-if="isVestsEmpty && !isLoading">
      <div class="empty-data mt-2">
        <img src="../assets/images/empty-data.svg" class="empty-data-image"/>
        <b-button 
        variant="outline-primary" 
        class="mt-2" 
        size="lg"
        to="/form"
        >Criar vestibular</b-button>
      </div>
    </template>
<template v-if="!isVestsEmpty && !isLoading">
  <div v-for="vest in vests" :key="vest.id">
      <b-card 
      :title="vest.initial" 
      class="mb-2"
      :class="{'finished-vest': isFinished(vest)}">
        <b-card-text>{{vest.name}}</b-card-text>
        <b-card-text>{{vest.description}}</b-card-text>

        <b-button variant="outline-secondary" class="mr-2" @click="UpdateStatus(vest.id,status.FINISHED)"
        v-b-tooltip.hover
        title="Concluir"><i class="fa-solid fa-check"></i></b-button>
        <b-button variant="outline-secondary" class="mr-2" @click="UpdateStatus(vest.id,status.ARCHIVED)" v-b-tooltip.hover
        title="Arquivar"><i class="fa fa-box-archive"></i></b-button>
        <b-button variant="outline-secondary" class="mr-2" @click="edit(vest.id)"
        v-b-tooltip.hover
        title="Editar"><i class="fa fa-edit"></i></b-button>
        <b-button variant="outline-danger" class="mr-2"  @click="remove(vest.id)"
        v-b-tooltip.hover
        title="Remover"><i class="fa fa-times"></i></b-button>
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
import VestsModel from "@/models/VestsModel";
import Status from "@/valueObjects/status.js";
import ToastMixin from '@/mixins/toastMixin.js';

export default {
  name: "List",
  mixins:[ToastMixin],
  data() {
    return {
      vests: [],
      vestSelected : [],
      status: Status,
      filter:{
        name:null,
        status:null
      },
      optionsList:[
        {value: null, text: "Selecione algum status" },
        {value: Status.OPEN, text: "Aberto" },
        {value: Status.FINISHED, text: "Concluído" },
        {value: Status.ARCHIVED, text: "Arquivado" }
      ],
      isLoading:false
      
    }
  },
  async created() {
    this.isLoading = true;
    this.vests = await VestsModel.params({status:[this.status.OPEN]}).get();
    this.isLoading = false;
  },

  methods:{
    edit(vestId){
      this.$router.push({name: "form", params: {vestId}});
    },
    async remove(vestId){
      this.vestSelected =  await VestsModel.find(vestId);
      this.$refs.modalRemove.show();
    },
    hideModal(){
      this.$refs.modalRemove.hide();
    },
    async confirmRemoveVest(){
      this.vestSelected.delete();
      // this.vests = await VestsModel.params(
      //   {status:
      //   [
      //     this.status.OPEN,
      //     this.status.FINISHED,
      //   ]
      //   }).get();
      this.vests = await VestsModel.params({status:[this.status.OPEN]}).get();
      this.hideModal();
    },
    async UpdateStatus(vestId,status){
      let vest = await VestsModel.find(vestId);
      vest.status = status;
      await vest.save();
      this.showToast("success", "Sucesso!", "Status do vestibular atualizado com sucesso");
      this.vests = await VestsModel.params({status:[this.status.OPEN]}).get();

      // this.vests = await VestsModel.params(
      //   {status:
      //   [
      //     this.status.OPEN,
      //     this.status.FINISHED,
      //   ]
      //   }).get();
    },
    isFinished(vest){
       if(vest.status === this.status.FINISHED) return true;
    },

    async filterVests(){
      let filter = {...this.filter}
      filter = this.clean(filter);
      this.vests = await VestsModel.params(filter).get();
    },

    clean(obj){
      for(var propName in obj){
        if(obj[propName] === null ||obj[propName] === undefined ){
          delete obj[propName];
        }
        return obj;
      }
    },
    async clearFilter(){
      this.filter = {
        name:null,
        status:null
      };
      this.vests = await VestsModel.params({status:[this.status.OPEN]}).get();
    }
  },

  computed:{
    isVestsEmpty(){
      return this.vests.length === 0;
    }
  }

}
</script>

<style scoped>
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

.finished-vest{
  opacity: 0.7;
}

.finished-vest > .card-body > h4
, .finished-vest > .card-body > p {
  text-decoration: line-through;
}

.loading-spin{
  display: flex;
  align-items: center;
  justify-content: center;
  height: 65vh;
}
</style>