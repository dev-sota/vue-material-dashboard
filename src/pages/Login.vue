<template>
  <div class="centered-container">
    <md-content class="md-elevation-3">
      <div class="title">
        <img src="@/assets/img/vue-logo.png" />
      </div>

      <div class="form">
        <md-field>
          <label>E-mail</label>
          <md-input v-model="login.email" autofocus></md-input>
        </md-field>

        <md-field md-has-password>
          <label>Password</label>
          <md-input v-model="login.password" type="password"></md-input>
        </md-field>
      </div>

      <div class="actions md-layout md-alignment-center-space-between">
        <a href="/resetpassword">Reset password</a>
        <md-button class="md-raised md-primary" @click="auth">Log in</md-button>
      </div>

      <div class="loading-overlay" v-if="loading">
        <md-progress-spinner
          md-mode="indeterminate"
          :md-stroke="2"
        ></md-progress-spinner>
      </div>
    </md-content>
    <div class="background" />
  </div>
</template>

<script>
import router from "@/routes/routes.js";

export default {
  name: "App",
  data() {
    return {
      loading: false,
      login: {
        email: "",
        password: ""
      }
    };
  },
  methods: {
    auth() {
      this.axios
        .get("https://jsondata.okiba.me/v1/json/Ia5JX200429010252")
        // .post("{{host}}/api/v1/sessions", {
        //   email: this.login.email,
        //   password: this.login.password
        // })
        .then(response => {
          this.$Cookies.set("accessToken", response.data.access_token, {
            expires: 5 / 1440 //5min
          });
          this.$Cookies.set(
            "accessTokenExpireDate",
            response.data.access_token_expire_date,
            {
              expires: 5 / 1440 //5min
            }
          );
          this.loading = true;
          setTimeout(() => {
            this.loading = false;
          }, 5000);
          router.push({ path: "/admin" });
        })
        .catch(error => {
          this.dangerNotify(
            "The login attempt failed. Either the user ID or password is invalid."
          );
        });
    }
  }
};
</script>

<style lang="scss">
.centered-container {
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  height: 100vh;
  .title {
    text-align: center;
    margin-bottom: 30px;
    img {
      margin-bottom: 16px;
      max-width: 80px;
    }
  }
  .actions {
    .md-button {
      margin: 0;
    }
  }
  .form {
    margin-bottom: 60px;
  }
  .background {
    position: absolute;
    height: 100%;
    width: 100%;
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;
    z-index: 0;
  }
  .md-content {
    z-index: 1;
    padding: 40px;
    width: 100%;
    max-width: 400px;
    position: relative;
  }
  .loading-overlay {
    z-index: 10;
    top: 0;
    left: 0;
    right: 0;
    position: absolute;
    width: 100%;
    height: 100%;
    background: rgba(255, 255, 255, 0.9);
    display: flex;
    align-items: center;
    justify-content: center;
  }
}
</style>
