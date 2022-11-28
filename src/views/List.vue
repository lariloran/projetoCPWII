<template>
  <div class="container mt-2">
    <div v-for="(vest, index) in vests" :key="index">
      <b-card :title="vest.subject" class="mb-2">
        <b-card-text>{{vest.description}}</b-card-text>

        <b-button variant="outline-secondary" class="mr-2" @click="edit(index)">Editar</b-button>
        <b-button variant="outline-danger" class="mr-2">Excluir</b-button>
      </b-card>
    </div>
  </div>
</template>



<script>
export default {
  name: "List",

  data() {
    return {
      vests: []
    }
  },
  created() {
    this.vests = (localStorage.getItem("vests")) ? JSON.parse(localStorage.getItem("vests")) : [];

  },

  methods:{
    edit(index){
      this.$router.push({name: "form", params: {index}});
    }
  }

}
</script>