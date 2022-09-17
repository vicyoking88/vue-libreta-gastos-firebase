<template>
  <div class="container py-4 form_login">
    <form @submit="entrar" method="post">
      <div class="row">
        <div class="me-auto ms-auto col-12 col-sm-9 col-md-7 col-lg-5">
          <h2>Administrar mis Gastos</h2>
          <hr />

          <label for="email">Correo Electrónico</label>
          <div class="input-group">
            <span class="input-group-text"><i class="fas fa-user"></i></span>
            <input
              v-model="email"
              type="text"
              name="email"
              id="email"
              class="form-control"
              placeholder="Email"
            />
          </div>

          <div id="errorEmail" class="error text-danger"></div>

          <label for="password">Contraseña</label>
          <div style="margin-bottom: 20px ;" class="input-group">
            <span class="input-group-text"><i class="fas fa-key"></i></span>
            <input
              v-model="password"
              type="password"
              name="password"
              id="password"
              class="form-control"
              placeholder="password"
            />
          </div>

          <div v-if="alert" class="alert alert-danger" role="alert">
            Contraseña y/o cuenta incorrecta
          </div>

          <div id="errorPassword" class="error text-danger mb-4"></div>

          <div class="row">
            <div class="col py-4">
              <button type="submit" class="btn btn-primary mb-2">
                Entrar
              </button>
            </div>
          </div>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: "loginForm",
  data: function() {
    return {
      email: "",
      password: "",
      alert: false,
    };
  },
  props: ["firebase"],
  methods: {
    entrar: function(e) {
      this.firebase
        .auth()
        .signInWithEmailAndPassword(this.email, this.password)
        .then((response) => {
          this.alert = false;
          console.log("acabas de autenticar con " + response.user.email);
          this.$emit("logueado", response.user.email);
        })
        .catch((error) => {
          this.alert = true;
          var erroCode = error.code;
          var errorMessange = error.message;
          console.log("Error: " + error.code + " - " + error.message);
          console.log(erroCode + "" + errorMessange);
        });
      e.preventDefault();
    },
  },
};
</script>

<style></style>
