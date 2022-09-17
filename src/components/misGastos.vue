<template>
  <div class="container">
    <div class="row">
      <h1 class="display-1 text-center col-sm-11">Mis GAstos</h1>
      <div class="col-sm-1 mt-5">
        <a class="icon-show" @click="mostrarAdd">
          <i v-bind:class="iconAdd"></i>
        </a>
      </div>
    </div>

    <div class="border px-4 py-4 my-4" v-if="mAdd">
      <form @submit="add">
        <div class="row mb-3">
          <label for="nGasto" class="col-sm-2 col-form-label">
            Nombre Del GAsto</label
          >
          <div class="col-sm-10">
            <input
              v-model="campo_nombre"
              placeholder="ingresa nuevo gasto"
              type="text"
              class="form-control"
              id="nGasto"
            />
          </div>
        </div>

        <div class="row mb-3">
          <label for="vMonto" class="col-sm-2 col-form-label">Monto</label>
          <div class="col-sm-10">
            <input
              v-model.number="campo_monto"
              placeholder="su monto"
              type="number"
              class="form-control"
              id="vMonto"
            />
          </div>
        </div>

        <div class="row mb-3">
          <label for="tGAsto" class="col-sm-2 col-form-label"
            >Tipo de Gasto</label
          >
          <div class="col-sm-10">
            <select
              v-model="campo_tipo"
              class="form-select form-control"
              aria-label="Default select example"
              id="tGasto"
            >
              <option selected>Escoge tipo de gasto</option>
              <option value="Hogar">Hogar</option>
              <option value="Vehiculos">Vehiculos</option>
              <option value="Salud">Salud</option>
              <option value="Trabajo">Trabajo</option>
            </select>
          </div>
        </div>

        <button type="submit" class="btn btn-primary">
          Agregar o Actualizar
        </button>
      </form>
    </div>

    <h2 class="mb-3">{{ usuario }}</h2>

    <!--MIS PESTAÑAS-->
    <ul class="nav nav-tabs" id="menu">
      <li class="nav-item" v-for="(tab, index) in tabs" :key="index">
        <a :class="tab.class" aria-current="page" @click="filtrar(tab.name)">{{
          tab.name
        }}</a>
      </li>
    </ul>

    <div class="my-2">
      <table class="table">
        <thead class="table-dark">
          <tr>
            <th scope="col">Nombre Gasto</th>
            <th scope="col">Monto Gasto</th>
            <th scope="col">Tipo GAsto</th>
            <th scope="col" class="text-center">Accion</th>
          </tr>
        </thead>

        <tbody>
          <tr v-for="(gasto, index) in gastos" :key="index" ref="cargaGastos">
            <td v-text="gasto.nombre" v-bind:name="gasto.nombre"></td>
            <td v-text="gasto.monto"></td>
            <td v-text="gasto.tipo"></td>
            <td class="text-center">
              <a
                class="icon-edit"
                @click="modificar(gasto.nombre, gasto.monto, gasto.tipo)"
              >
                <i class="fas fa-edit fa-2x me-3"></i>
              </a>
              <a class="icon-delete" @click="borrar(gasto.nombre)">
                <i class="fas fa-trash-alt fa-2x ms-2"></i>
              </a>
            </td>
          </tr>
          <tr>
            <td>Total</td>
            <td colspan="3">{{ total }}</td>
          </tr>
        </tbody>
      </table>
    </div>

    <div class="text-center mt-5">
      <button type="button" class="btn btn-danger" @click="salir">
        Salir De mis Gastos
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "misGastos",
  data: function() {
    return {
      mAdd: false,
      mensaje: "cerro sesion",
      iconAdd: "fas fa-plus fa-2x",
      campo_nombre: "",
      campo_monto: "",
      campo_tipo: "",
      nombre_modif: "",
      monto_modif: "",
      tipo_modif: "",
      filtro: this.gastos.gastos,
    };
  },
  props: {
    usuario: String,
    gastos: Array,
    total: Number,
    tabs: Array,
  },

  methods: {
    filtrar: function(tipo_filtro) {
      this.$emit("filtrar_hijo", tipo_filtro);
    },
    add: function(e) {
      let newG = [this.campo_nombre, this.campo_monto, this.campo_tipo];
      this.$emit("add", newG);
      e.preventDefault();
    },
    salir: function() {
      this.$emit("salir");
    },
    borrar: function(aBorrar) {
      this.$emit("borrar_hijo", aBorrar);
    },
    modificar: function(nombre, monto, tipo) {
      this.campo_nombre = nombre;
      this.campo_monto = monto;
      this.campo_tipo = tipo;
    },
    mostrarAdd: function() {
      if (this.mAdd) {
        this.mAdd = false;
        this.iconAdd = "fas fa-plus fa-2x";
      } else {
        this.mAdd = true;
        this.iconAdd = "fas fa-minus fa-2x";
      }
    },
  },
};
</script>

<style>
.icon-show,
.icon-delete,
.icon-edit,
.nav-item {
  cursor: pointer;
}
</style>
