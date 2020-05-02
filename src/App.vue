<template>
  <div v-if="logon" id="app" class="container">
    <!-- Contenido de las listas de gasto -->
    <div class="row text-primary">
      <div class="col-10">
        <h1>Lista de Gastos</h1>
      </div>
      <div class="col-2">
        <span
          v-bind:id="'agregar'"
          v-bind:class="['mr-1', 'btn', 'btn-primary', boton]"
          style="font-size: 10px; margin-top: 15px"
          v-on:click="butonPlus($event)"
        ></span>
      </div>
    </div>
    <div
      v-if="despliegue"
      key="despliegue"
      class="container border rounded text-primary lead p-2 mt-3"
    >
      <div class="row justify-content-md-center">
        <div class="col-3"></div>
        <div class="col-3">Nombre del gasto</div>
        <div class="col-3">
          <input v-model="nombreGasto" class="text-primary" type="text" />
        </div>
        <div class="col-3"></div>
      </div>
      <div class="row justify-content-md-center my-2">
        <div class="col-3"></div>
        <div class="col-3">Monto del gasto</div>
        <div class="col-3">
          <input v-model="montoGasto" class="text-primary" type="text" />
        </div>
        <div class="col-3"></div>
      </div>
      <div class="row justify-content-md-center my-2">
        <div class="col-3"></div>
        <div class="col-3">Tipo de gasto</div>
        <div class="col-3">
          <input v-model="tipoGasto" class="text-primary" type="text" />
        </div>
        <div class="col-3"></div>
      </div>
      <div class="row justify-content-md-center my-2">
        <div v-if="actualizar" class="col-12">
          <div id="actualizar" class="btn btn-primary" v-on:click="manejarClick($event)">Actualizar</div>
        </div>
        <div v-else class="col-12">
          <div id="agregar" class="btn btn-primary" v-on:click="manejarClick($event)">Agregar</div>
        </div>
      </div>
    </div>
    <!-- filtros -->
    <div class="container border rounded text-primary mt-4">
      <div class="row lead border rounded font-weight-bold">
        <div class="col-3">Nombre del gasto</div>
        <div class="col-3">Monto del gasto</div>
        <div class="col-3">Tipo de gasto</div>
        <div class="col-3">Accion</div>
      </div>
      <!-- lista de los gastos -->
    </div>
  </div>
  <div v-else>
    <!-- Contenido de login -->
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
      boton: "fa fa-plus",
      gastos: [],
      filtrados: [],
      coleccion: {},
      logon: false,
      firebase: "",
      idUsuario: "",
      db: "",
      actualizar: false,
      despliegue: false
    };
  },
  methods: {
    butonPlus: function(event) {
      if (event.target.id === "agregar") {
        this.boton = "fa fa-minus";
        document.getElementById("agregar").setAttribute("id", "actualizar");
        this.despliegue = true;
      } else if (event.target.id === "actualizar") {
        this.boton = "fa fa-plus";
        document.getElementById("actualizar").setAttribute("id", "agregar");
        this.despliegue = false;
        this.actualizar = false;
      }
    },
    manejarClick: function(evento) {
      if (evento.target.id === "agregar") {
        // Se agregara los nuevos gastos

        this.despliegue = false;
        this.boton = "fa fa-plus";
        document.getElementById("actualizar").setAttribute("id", "agregar");
      }
      if (evento.target.id === "actualizar") {
        // Actualizando los gastos

        this.despliegue = false;
        this.actualizar = false;
        this.boton = "fa fa-plus";
        document.getElementById("actualizar").setAttribute("id", "agregar");
      }
    },
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
          this.totalGasto += parseFloat(gasto.data().MontoGasto);
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
      authDomain: "vue-practica-f8ec.firebaseapp.com",
      databaseURL: "https://vue-practica-f8ecc.firebaseio.com",
      projectId: "vue-practica-f8ecc",
      storageBucket: "vue-practica-f8ecappspot.com",
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
