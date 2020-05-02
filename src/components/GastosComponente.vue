<template>
  <div>
    <div class="row lead my-1">
      <div class="col-3">{{gasto.nombre}}</div>
      <div class="col-3">{{gasto.monto}}</div>
      <div class="col-3">{{gasto.tipo}}</div>
      <div class="col-1">
        <div
          id="editar"
          class="btn btn-primary"
          v-on:click="editar($event,{
            indice:indice, 
            id:id, 
            nombre: gasto.nombre,
            monto: gasto.monto,
            tipo: gasto.tipo,
            mostrar: true
          })"
        >editar</div>
      </div>
      <div class="col-2">
        <div
          id="eliminar"
          class="btn btn-primary"
          v-on:click="manejarClick($event,{indice:indice, id:id, monto: gasto.monto})"
        >Eliminar</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "gastos",
  props: ["gasto", "id", "indice"],
  data: function() {
    return {
      test: "test",
      monstrar: false
    };
  },
  methods: {
    manejarClick: function(evento, gastoID) {
      if (evento.target.id === "eliminar") {
        this.$emit("eliminarGasto", gastoID);
      }
    },
    editar: function(event, cambios) {
      this.$emit("editarGasto", cambios);
      //   this.monstrar = true;
    },
    agregarTarea: function(event) {
      console.log(event.mostrar);
      this.$emit("editarGasto", event);
      if (!event.mostrar) this.monstrar = false;
    }
  }
};
</script>

<style>
</style>
