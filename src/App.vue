<template>
  <div id="app">
    <b-navbar toggleable="lg" type="dark" variant="dark">
    <b-navbar-brand to="/">Portal Gerência</b-navbar-brand>

    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

    <b-collapse id="nav-collapse" is-nav>
      <b-navbar-nav>
        <b-nav-item to="/">Início</b-nav-item>
        <b-nav-item to="/form">Criar Vestibular</b-nav-item>
      </b-navbar-nav>
    </b-collapse>
    
    </b-navbar>

    <Transition name="fade" mode="out-in">
      <router-view/>
    </Transition>
  </div>
</template>

<style>
.fade-enter-active, .fade-leave-active{
  transition-duration: 0.2s;
  transition-property: opacity;
  transition-timing-function: ease;
}

.fade-enter, .fade-leave{
  opacity: 0;
}
</style>
