<template>
  <div class="container">
    <h2>Iniciar sesión</h2>
    <form @submit="login">
      <div class="form-group">
        <label for="username">Usuario:</label>
        <input
          type="text"
          id="username"
          v-model="username"
          class="input-field"
        />
        <span v-if="errors.username" class="error-message">{{
          errors.username
        }}</span>
      </div>
      <div class="form-group">
        <label for="password">Contraseña:</label>
        <input
          type="password"
          id="password"
          v-model="password"
          class="input-field"
        />
        <span v-if="errors.password" class="error-message">{{
          errors.password
        }}</span>
      </div>
      <div class="form-group">
        <label for="key">Clave:</label>
        <input type="text" id="key" v-model="key" class="input-field" />
        <span v-if="errors.key" class="error-message">{{ errors.key }}</span>
      </div>
      <button type="submit" class="btn-submit">Iniciar sesión</button>
    </form>

    <!-- Mostrar mensaje de error -->
    <p v-if="errorMessage" class="error-message">{{ errorMessage }}</p>

    <!-- Mostrar mensaje de éxito -->
    <p v-if="successMessage" class="success-message">{{ successMessage }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      username: "",
      password: "",
      key: "",
      errorMessage: "",
      successMessage: "",
      errors: {
        username: "",
        password: "",
        key: "",
      },
    };
  },
  methods: {
    login(event) {
      event.preventDefault();

      this.clearErrors();

      if (!this.username) {
        this.errors.username = "Por favor, ingresa tu usuario.";
      }
      if (!this.password) {
        this.errors.password = "Por favor, ingresa tu contraseña.";
      }
      if (!this.key) {
        this.errors.key = "Por favor, ingresa tu clave.";
      }

      if (this.hasErrors()) {
        this.errorMessage =
          "Por favor, corrige los errores y completa todos los campos.";
        this.successMessage = "";
      } else {
        const url = "https://jacgsaw.com/auth/login";
        const formData = new URLSearchParams();
        formData.append("user", this.username);
        formData.append("pacc", this.password);
        formData.append("key", this.key);

        fetch(url, {
          method: "POST",
          body: formData,
        })
          .then((response) => response.json())
          .then((data) => {
            if (data.sys.login === true) {
              this.successMessage = "Inicio de sesión exitoso";
              this.errorMessage = "";
            } else {
              this.successMessage = "";
              this.errorMessage =
                "Error en el inicio de sesión. Por favor, verifica los datos.";
            }
            console.log(data.login);
          })
          .catch((error) => {
            console.error(error);
            this.successMessage = "";
            this.errorMessage =
              "Error en el inicio de sesión. Por favor, inténtalo nuevamente más tarde.";
          });
      }
    },
    clearErrors() {
      this.errors.username = "";
      this.errors.password = "";
      this.errors.key = "";
      this.errorMessage = "";
      this.successMessage = "";
    },
    hasErrors() {
      return this.errors.username || this.errors.password || this.errors.key;
    },
  },
};
</script>

<style>
.container {
  max-width: 400px;
  margin: 0 auto;
}

.form-group {
  margin-bottom: 20px;
}

.input-field {
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  width: 100%;
}

.btn-submit {
  background-color: #e53935;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

.error-message {
  color: red;
  margin-top: 10px;
}

.success-message {
  color: green;
  margin-top: 10px;
}
</style>
