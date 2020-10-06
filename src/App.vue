<template>
  <div id="app">
      <b-navbar toggleable="md" type="dark" variant="info">
        <b-container>
          <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
          <b-navbar-brand href="#">Ponchar</b-navbar-brand>
          <b-collapse id="nav-collapse" is-nav>
            <b-navbar-nav>
              <b-nav-item :to="{name: 'PoncharEntrada'}">Ponchar Entrada</b-nav-item>
              <b-nav-item :to="{name: 'OtrosPonches'}">Otros Ponches</b-nav-item>
              <b-nav-item :to="{name: 'Historial'}">Historial</b-nav-item>
              <b-nav-item :to="{name: 'Empleado'}">Empleados</b-nav-item>
            </b-navbar-nav>
          </b-collapse>
        </b-container>
      <!--<router-link to="/ponchar-entrada">Ponchar Entrada</router-link> |
      <router-link to="/otros-ponches">Otros Ponches</router-link> |
      <router-link to="/Historial">Historial</router-link> |
      <router-link to="/empleados">Empleados</router-link>-->
      </b-navbar>
      <b-container>
        <router-view/>
      </b-container>
  </div>
</template>

<style>
.hasError label {
  color: red;
}
</style>
