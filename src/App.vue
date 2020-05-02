<template>
  <div v-if="logon" id="app" class="container">
    <!-- Contenido de las listas de gasto -->
    <h1>hola mundo</h1>
  </div>
  <div v-else>
    <loginForm v-bind:firebase="firebase" v-on:ingresoCorrecto="ingresoCorrecto($event)"></loginForm>
  </div>
</template>

<script>
import firebase from "firebase";
import "firebase/firestore";
import loginForm from "./components/loginForm.vue";

export default {
  name: "app",
  data: function() {
    return {
      gastos: [],
      coleccion: {},
      logon: false,
      firebase: "",
      idUsuario: "",
      db: ""
    };
  },
  methods: {
    ingresoCorrecto: function(usuario) {
      console.log("User: " + usuario);
      this.idUsuario = usuario;
      this.logon = true;
      this.coleccion = this.db.collection("/Usuarios/" + usuario + "/Gastos");
      this.coleccion.get().then(gastos => {
        gastos.forEach(gasto => {
          this.gastos.push({
            id: gasto.id,
            monto: gasto.data().MontoGasto,
            nombre: gasto.data().NombreGasto,
            tipo: gasto.data().TipoGasto
          });
        });
      });
    }
  },
  components: {
    loginForm
  },
  beforeMount: function() {
    var config = {
      apiKey: " AIzaSyCMnEqx827ZNmelgPgL0gC81DhSIXBLs6s",
      authDomain: "vue-practica-f8ecc.firebaseapp.com",
      databaseURL: "https://vue-practica-f8ecc.firebaseio.com",
      projectId: "vue-practica-f8ecc",
      storageBucket: "vue-practica-f8ecc.appspot.com",
      messagingSenderId: "310273136041"
    };
    firebase.initializeApp(config);
    this.db = firebase.firestore();
    const settings = { timestampsInSnapshots: true };
    this.db.settings(settings);
    this.firebase = firebase;
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
