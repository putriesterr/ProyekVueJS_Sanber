<template>
  <v-card>
    <v-toolbar dark>
      <v-btn icon dark @click.native="close">
        <v-icon>mdi-close</v-icon>
      </v-btn>
      <v-toolbar-title>Register</v-toolbar-title>
    </v-toolbar>

    <v-container fluid>
      <v-form ref="form">
        <v-text-field 
        v-model="name" 
        label="Name" 
        required></v-text-field>
        <v-text-field 
        v-model="email" 
        label="E-mail" 
        required 
        append-icon="mdi-email"></v-text-field>
        <v-text-field 
        v-model="password" 
        label="Password"
        required 
        :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'" 
        :type="showPassword ? 'text' : 'password'" 
        @click:append="showPassword = !showPassword"></v-text-field>

        <div class="text-xs-center">
          <center>
            <v-btn dark @click="submit">
              Register
              <v-icon right dark>mdi-lock-open</v-icon>
            </v-btn>
          </center>
        </div>
      </v-form>
    </v-container>
  </v-card>
</template>

<script>
import { mapActions } from "vuex";
export default {
  data() {
    return {
      email: '',
      name: '',
      showPassword: false,
      password: '',
      apiDomain: "https://demo-api-vue.sanbercloud.com",
    };
  },
  methods: {
    ...mapActions({
      setAlert: "alert/set",
      setUser: "auth/setUser",
    }),
    close() {
      this.$emit("closed", false);
    },
    submit() {
      const config = {
        method: "post",
        url: this.apiDomain + "/api/v2/auth/register",
        data: {
          name: this.name,
          email: this.email,
          password: this.password,
        },
      };

      this.axios(config)
        .then(response => {
          this.setUser(response.data.name);
          this.setUser(response.data.email);

          this.setAlert({
            status: true,
            color: "success",
            text: "Anda Berhasil Register",
          });
          this.close();
        })
        .catch(response => {
          console.log(response);

          this.setAlert({
            status: true,
            color: "error",
            text: "Anda Gagal Register",
          });
        });
    },
  },
};
</script>
