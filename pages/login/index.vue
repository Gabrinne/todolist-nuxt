<template>
  <v-main>
    <v-container class="fill-height d-flex justify-center align-center" fluid>
      <v-form
        ref="form"
        v-model="valid"
        lazy-validation
        @submit.prevent="submit"
      >
        <v-card class="elevation-4" width="350px">
          <v-toolbar color="primary" dark flat>
            <v-toolbar-title v-if="user.name === ''">Login</v-toolbar-title>
            <v-toolbar-title v-else>{{ user.name }}</v-toolbar-title>
          </v-toolbar>
          <v-card-text>
            <v-alert type="error" v-if="validUser">{{ errorUser }}</v-alert>
            <v-text-field
              label="Login"
              name="login"
              prepend-icon="mdi-account"
              type="text"
              v-model="form.username"
              @blur="username"
              counter="8"
              :rules="rules.username"
            ></v-text-field>

            <v-text-field
              id="password"
              label="Password"
              name="password"
              prepend-icon="mdi-lock"
              type="password"
              v-model="form.password"
              counter="8"
              :rules="rules.password"
            ></v-text-field>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="primary" type="submit">Login</v-btn>
          </v-card-actions>
        </v-card>
      </v-form>
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

      rules: {
        password: [
          v => !!v || "Senha é obrigatória",
          v =>
            (v && v.length >= 8) ||
            "O campo password deve possuir no mínimo 8 caracteres"
        ],
        username: [
          v => !!v || "Nome é obrigatório",
          v =>
            (v && v.length >= 8) ||
            "O campo login deve possuir no mínimo 8 caracteres "
        ]
      },
      user: {
        name: "",
        avatar: ""
      },
      validUser: false,
      errorUser: null,
      valid: true
    }
  },
  methods: {
    username() {
      if (this.form.username !== "") {
        this.$axios
          .post(`${process.env.apiIdentity}/v1/auth/user`, this.form)
          .then(res => {
            console.log(res)
            this.user = res.data
          })
          .catch(error => {
            console.log(error.response.data)
            this.errorUser = error.response.data
            this.validUser = true
          })
      }
    },

    submit() {
      if (this.$refs.form.validate()) {
        this.$axios
          .post(`${process.env.apiIdentity}/v1/auth/login`, this.form)
          .then(res => {
            console.log(res)
          })
          .catch(error => {
            console.log(error)
          })
      }
    }
  }
}
</script>

<style></style>
