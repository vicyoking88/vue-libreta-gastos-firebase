<template>
  <div id="app">
    <div id="login" v-if="!logon">
      <loginForm v-bind:firebase="firebase" v-on:logueado="logueado($event)">
      </loginForm>
    </div>
    <div id="logueado" v-else>
      <misGastos
        v-bind:usuario="usuario"
        v-bind:gastos="filtro"
        v-bind:total="total"
        v-bind:tabs="tabs"
        v-on:salir="salir($event)"
        v-on:add="add($event)"
        v-on:borrar_hijo="borrar_padre($event)"
        v-on:filtrar_hijo="filtrar_padre($event)"
      ></misGastos>
    </div>
  </div>
</template>

<script>
import firebase from "firebase";
import "firebase/firestore";

import loginForm from "./components/loginForm.vue";
import misGastos from "./components/misGastos.vue";

//import HelloWorld from './components/HelloWorld.vue'

export default {
  name: "App",
  data: function() {
    return {
      logon: false,
      firebase: "",
      db: "",
      gastos: [],
      total: 0,
      sumat: 0,
      datosUser: {},
      usuario: "",
      tipos: ["Todo"],
      tabsR: [],
      tabs: [],
      filtro: [],
      tipo: "Todo",
    };
  },
  components: {
    loginForm,
    misGastos,
  },

  // Your web app's Firebase configuration
  // For Firebase JS SDK v7.20.0 and later, measurementId is optional
  beforeMount: function() {
    var firebaseConfig = {
      apiKey: "AIzaSyBZ3Jr8-LzGYe316P6i8nwejxxYwdbywis",
      authDomain: "libreta-gastos.firebaseapp.com",
      projectId: "libreta-gastos",
      storageBucket: "libreta-gastos.appspot.com",
      messagingSenderId: "341147965806",
      appId: "1:341147965806:web:08b25f79663fb86c19097f",
      measurementId: "G-9MG06YP628",
    };
    // Initialize Firebase
    firebase.initializeApp(firebaseConfig);
    this.db = firebase.firestore();
    this.firebase = firebase;
  },
  methods: {
    logueado: function(user) {
      this.sumat = 0;
      this.total = 0;
      this.usuario = user;
      /**consultar la coleccion de gastos */
      this.db
        .collection("usuarios")
        .doc(user)
        .collection("gastos")
        .get()
        .then((gastos) => {
          gastos.forEach((gasto) => {
            this.gastos.push({
              nombre: gasto.id,
              tipo: gasto.data().tipo,
              monto: gasto.data().monto,
            });

            /**cargue de pestañas */
            this.tipos.push(gasto.data().tipo);

            /**filtro por tipo */
            if (this.tipo != "Todo") {
              if (gasto.data().tipo == this.tipo) {
                this.filtro.push({
                  nombre: gasto.id,
                  tipo: gasto.data().tipo,
                  monto: gasto.data().monto,
                });

                this.total = gasto.data().monto + this.total;
              }
            }
          });
          gastos.forEach(
            (element) => (this.sumat = this.sumat + element.data().monto)
          );

          if (this.tipo == "Todo") {
            this.filtro = this.gastos;
            this.total = this.sumat;
          }

          this.tabs = [];
          /**convertimos en un arreglo para las pestañas */
          this.tabsR = this.tipos.filter((item, index) => {
            return this.tipos.indexOf(item) === index;
          });
          this.tabsR.forEach((tab) => {
            this.tabs.push({ name: tab, class: "nav-link" });
          });
        });
      /**si no hay filtro que aplicar */
      this.logon = true;
    },

    filtrar_padre: function(tipo_filtro) {
      this.tipo = tipo_filtro;
      this.gastos = [];
      this.filtro = [];
      this.logueado(this.usuario);
    },

    salir: function() {
      this.logon = false;
      this.gastos = [];
      this.filtro = [];
    },
    add: function(newG) {
      this.db
        .collection("usuarios")
        .doc(this.usuario)
        .collection("gastos")
        .doc(newG[0])
        .set({
          tipo: newG[2],
          monto: newG[1],
        })
        .then(() => {
          console.log("Document successfully written! ");
          this.gastos = [];
          this.filtro = [];
          this.datosUser = {};

          this.logueado(this.usuario);
        })
        .catch((error) => {
          console.error("Error adding document: ", error);
        });
    },
    borrar_padre: function(aborrar) {
      console.log("entro a borrar");
      console.log(aborrar);
      this.db
        .collection("usuarios")
        .doc(this.usuario)
        .collection("gastos")
        .doc(aborrar)
        .delete()
        .then(() => {
          console.log("Document successfully deleted!");
          this.gastos = [];
          this.filtro = [];
          this.logueado(this.usuario);
        })
        .catch((error) => {
          console.error("Error removing document: ", error);
        });
    },
  },
};
</script>
<style></style>
