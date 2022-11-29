<template>
  <div class="container mt-2">
    <b-form>

      <b-form-group label="Nome" label-for="name">
        <b-form-input 
        id="subject"
        v-model="form.name"
        type="text"
        placeholder="Ex: Vestibular 2023"
        required
        autocomplete="off"></b-form-input>
      </b-form-group>

      <b-form-group label="Sigla" label-for="initial">
        <b-form-input 
        id="subject"
        v-model="form.initial"
        type="text"
        placeholder="Ex: VEST23-1"
        required
        autocomplete="off"></b-form-input>
      </b-form-group>

      <b-form-group label="Número edital" label-for="edital">
        <b-form-input 
        id="subject"
        v-model="form.edital"
        type="text"
        placeholder="Ex: 45/2023"
        required
        autocomplete="off"></b-form-input>
      </b-form-group>

      <b-form-group label="Data início" label-for="dat_ini">
        <b-form-input 
        id="subject"
        v-model="form.dat_ini"
        type="date"
        required
        autocomplete="off"></b-form-input>
      </b-form-group>

      <b-form-group label="Data fim" label-for="dat_end">
        <b-form-input 
        id="subject"
        v-model="form.dat_end"
        type="date"
        required
        autocomplete="off"></b-form-input>
      </b-form-group>

      <b-form-group label="Descrição(opcional)" label-for="description">
        <b-form-textarea 
        id="description"
        v-model="form.description"
        type="text"
        placeholder="Ex: Vestibular 2023 acontecerá no dia XX.XX.XXXX às 14:00 horas"
        autocomplete="off"></b-form-textarea>
      </b-form-group>

      <b-button type="submit" variant="outline-primary" @click="saveVestibular">
          Salvar
      </b-button>
    </b-form>
</div>
</template>

<script>
import ToastMixin from '@/mixins/toastMixin.js';

  export default{
    name: "Form",
    mixins:[ToastMixin],
    data(){
      return{
        form:{
        name:"",
        initial:"",
        edital:"",
        dat_ini:"",
        dat_end: "",
        description: ""
      },
      methodSave : "new"
      }
    },

    created(){
      if(this.$route.params.index === 0 || this.$route.params.index !== undefined){
        this.methodSave = "update";
        let vests = JSON.parse(localStorage.getItem("vests"));
        this.form = vests[this.$route.params.index];
      }
    },

    methods:{
      saveVestibular(){
        if(this.methodSave === "update"){
          let vests = JSON.parse(localStorage.getItem("vests"));
          vests[this.$route.params.index] = this.form;
          localStorage.setItem("vests", JSON.stringify(vests));
        this.showToast("success", "Sucesso!", "Tarefa atualizada com sucesso");
          this.$router.push({name:"list"});
          return;
        }
        let vests = (localStorage.getItem("vests")) ? JSON.parse(localStorage.getItem("vests")) : [];
        vests.push(this.form);
        localStorage.setItem("vests", JSON.stringify(vests));
        this.showToast("success", "Sucesso!", "Vestibular criado com sucesso");

        this.$router.push({name:"list"});
      }

    }
  }
</script>
