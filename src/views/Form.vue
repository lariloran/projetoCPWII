<template>
  <div class="container mt-2">
    <b-form>

      <b-form-group label="Nome" label-for="name">
        <b-form-input 
        id="name"
        v-model.trim="$v.form.name.$model"
        type="text"
        placeholder="Ex: Vestibular 2023"
        required
        autocomplete="off"         
        :state="nameValidation"
        aria-describedby="name-feedback">
      </b-form-input>
      <b-form-invalid-feedback id="name-feedback">Campo obrigatório</b-form-invalid-feedback>
      </b-form-group>

      <b-form-group label="Sigla" label-for="initial">
        <b-form-input 
        id="initial"
        v-model.trim="$v.form.initial.$model"
        type="text"
        placeholder="Ex: VEST23-1"
        required
        autocomplete="off"
        :state="initialValidation"></b-form-input>
      </b-form-group>

      <b-form-group label="Número edital" label-for="edital">
        <b-form-input 
        id="edital"
        v-model.trim="$v.form.edital.$model"
        type="text"
        placeholder="Ex: 45/2023"
        required
        autocomplete="off"
        :state="editalValidation"></b-form-input>
      </b-form-group>

      <b-form-group label="Data início" label-for="dat_ini">
        <b-form-input 
        id="dat_ini"
        v-model.trim="$v.form.dat_ini.$model"
        type="date"
        required
        autocomplete="off"
        :state="dat_iniValidation"></b-form-input>
      </b-form-group>

      <b-form-group label="Data fim" label-for="dat_end">
        <b-form-input 
        id="dat_end"
        v-model.trim="$v.form.dat_end.$model"
        type="date"
        required
        autocomplete="off"
        :state="dat_endValidation"></b-form-input>
      </b-form-group>

      <b-form-group label="Descrição(opcional)" label-for="description">
        <b-form-textarea 
        id="description"
        v-model="form.description"
        type="text"
        placeholder="Ex: Vestibular 2023 acontecerá no dia XX.XX.XXXX às 14:00 horas"
        autocomplete="off"></b-form-textarea>
      </b-form-group>
      <b-button
       type="submit" 
       variant="outline-primary" 
       @click="saveVestibular"
       >
          Salvar
      </b-button>

    </b-form>
</div>
</template>

<script>
import ToastMixin from '@/mixins/toastMixin.js';
import {required, minLength} from "vuelidate/lib/validators";
var i = 0;
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

    validations: {
      form: {
        name:{
          required,
          minLength: minLength(3)
        },
        initial:{
          required,
          minLength: minLength(3)
        },
        edital:{
          required
        },
        dat_ini:{
          required
        },
        dat_end:{
          required
        },
    
      }
    } ,
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

    },
    computed:{
      nameValidation(){
        if(this.$v.form.name.$dirty === false){
          return null;
        }
        else{
          i++;
          return !this.$v.form.name.$error;
        }
     
      },
      initialValidation(){
        if(this.$v.form.initial.$dirty === false){
          return null;
        }
        else{
          i++;
          return !this.$v.form.initial.$error;
        }
      },
      editalValidation(){
        if(this.$v.form.edital.$dirty === false){
          return null;
        }
        else{
          i++;
          return !this.$v.form.edital.$error;
        }
      },
      dat_iniValidation(){
        if(this.$v.form.dat_ini.$dirty === false){
          return null;
        }
        else{
          i++;
          return !this.$v.form.dat_ini.$error;
        }
      },
      dat_endValidation(){
        if(this.$v.form.dat_end.$dirty === false){
          return null;
        }
        else{
          i++;
          return !this.$v.form.dat_end.$error;
        }
      },
    }
  }
</script>
