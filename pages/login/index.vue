<template>
  <v-main>
    <v-container class="fill-height d-flex justify-center align-center" fluid>
      <v-card class="elevation-4" width="350px">
        <v-toolbar color="primary" dark flat>
          <v-toolbar-title v-if="user.name === '' ">Login</v-toolbar-title>
          <v-toolbar-title v-else>{{ user.name }}</v-toolbar-title>
        </v-toolbar>
        <v-card-text>
          <v-form>
            <v-alert type="error" v-if="validUser">{{ errorUser }}</v-alert>
            <v-text-field
              label="Login"
              name="login"
              prepend-icon="mdi-account"
              type="text"
              v-model="form.username"
              @blur="username"
            ></v-text-field>

            <v-text-field
              id="password"
              label="Password"
              name="password"
              prepend-icon="mdi-lock"
              type="password"
              v-model="form.password"
            ></v-text-field>
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" @click="submit">Login</v-btn>
        </v-card-actions>
      </v-card>
    </v-container>
  </v-main>
</template>

<script>
export default {
  layout: "simple",
  data() {
    return {
      form: {
        username: "",
        password: ""
      },
      user: {
        name: "",
        avatar: ""
      },
      validUser: false,
      errorUser: null
    };
  },
  methods: {
    username() {
      this.$axios
        .post(
          "https://development-identity-api.althaia.com.br/v1/auth/user",
          this.form
        )
        .then(res => {
          console.log(res);
          this.user = res.data;
        })
        .catch(error => {
          console.log(error.response.data);
          this.errorUser = error.response.data;
          this.validUser = true;
        });
    },

    submit() {
      this.$axios.post(
        "https://development-identity-api.althaia.com.br/v1/auth/user",
        this.form
      );
    }
  }
};
</script>

<style>
</style>
