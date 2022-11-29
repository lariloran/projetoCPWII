<template>
  <div class="container mt-2">
    <b-form>

      <b-form-group label="Título" label-for="subject">
        <b-form-input 
        id="subject"
        v-model="form.subject"
        type="text"
        placeholder="Ex: Vestibular 2023"
        required
        autocomplete="off"></b-form-input>
      </b-form-group>


      <b-form-group label="Descrição" label-for="description">
        <b-form-textarea 
        id="description"
        v-model="form.description"
        type="text"
        placeholder="Ex: Vestibular 2023 acontecerá no dia XX.XX.XXXX"
        required
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
        subject: "",
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
        this.showToast("success", "Sucesso!", "Tarefa criada com sucesso");

        this.$router.push({name:"list"});
      }

    }
  }
</script>
