
<template>
  <v-app style="background: linear-gradient(to left, '#C4C4C4', '#F0F0F0');">
    <v-container class="fill-height" fluid>
      <v-row align="center" justify="center">
        <v-col cols="12"
              sm="8"
              md="4">
            <v-card class="mx-auto" shaped elevation="20">

            <v-toolbar color="#BDB873" dark flat>
              <v-toolbar-title class="headline">登入..頁面</v-toolbar-title>
            </v-toolbar>

            <v-card-text>
                <v-form ref="form" v-bind:disabled="disabled" lazy-validation>
                  <v-text-field type="text" v-model="email"  label="帳號" :rules="emailRules" prepend-icon="mdi-at" /> 
                  <v-text-field :type="showPassword ? 'text' : '密碼'" v-model="password"  label="密碼" :rules="passwordRules" prepend-icon="mdi-lock" :append-icon="showPassword ? 'mdi-eye-off' : 'mdi-eye'"
                            @click:append="showPassword = !showPassword" /> 
              </v-form>

            <p class="red--text">{{ error }} </p>

            </v-card-text>

            <v-card-actions align="center" justify="center">
              <v-col>
                 <v-btn
                    outlined
                    color="#7F7D63"
                    text
                    to="/"
                  >
                    返回首頁
                  </v-btn> 
              </v-col> 


              <v-col>
                 <v-btn
                    outlined
                    color="#7F7D63"
                    text
                    to="/fgtpw"
                  >
                    忘記密碼
                  </v-btn> 
              </v-col> 

              <!-- <v-col>
                 <v-btn
                    outlined
                    color="#7F7D63"
                    text
                    @click="signUp"
                  >
                    signUp
                  </v-btn>  
              </v-col>  -->


              <v-col>
                <v-btn dark v-bind:disabled="disabled"  block @click="validate" color="#7F7D63">
                  <span v-show="!disabled">登入</span>
                  <span v-show="disabled">
                    <v-progress-circular indeterminate color="#7F7D63"></v-progress-circular>
                  </span>
                </v-btn> 
              </v-col>
              
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>
import firebase from 'firebase';

export default {
  data() {
    return {
      disabled: false,
      email: '',
      emailRules: [
        v => !!v || 'E-mail is required',
        v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
      ],
      showPassword: false,
      password: '',
      passwordRules: [
        v => !!v || 'Password is required'
      ],
      error: ''
    }
  },
  methods: {
//     signUp() {
//   firebase
//     .firebase
//     .auth()
//     .createUserWithEmailAndPassword(this.email, this.password)
//     .then(() => {
//       // now we have access to the signed in user
//       const user = firebase.auth().currentUser;
//       // send the signed in user a verification email
//       user.sendEmailVerification();
//     })
//     .catch(error => {
//       // catch any errors
//       console.log(error);
//     });
// },
    validate() {
      if(this.$refs.form.validate()) {
        this.disabled = true;

        firebase
            .auth()
            .signInWithEmailAndPassword(this.email, this.password)
            .then(() => {
              this.disabled = false;
              this.$router.replace({ path: "/Blog_v1" });
            })
            .catch(err => {
              this.disabled = false;
              this.error = err.message;
            });
      }
    }
  }
}
</script>
