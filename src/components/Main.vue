<template>
    <v-container>
      <v-row>
        <v-col cols="1">
        </v-col>
        <v-col cols="10">
          <h3 color="red">Welcome {{userName}} </h3>
        </v-col>
        <v-col cols="1">
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="11">
          <v-sheet>
            <v-form fast-fail>
              <v-text-field
                v-model="userNm"
                label="Username"
              ></v-text-field>
              <v-text-field
                :append-icon="pwShow ? 'mdi-eye' : 'mdi-eye-off'"
                :type="pwShow ? 'text' : 'password'"
                v-model="password"
                name="input-10-2"
                label="Password"
                hint="At least 8 characters"
                class="input-group--focused"
                @click:append="pwShow = !pwShow"
              ></v-text-field>
              <v-row>
                <v-col cols="10">
                    <v-btn @click="login" block class="mt-2" color="primary">Login</v-btn>
                </v-col>
                <v-col cols="2">
                  <v-btn type="submit" block class="mt-2" color="">
                    <v-icon
                      color="red-darken-2"
                      icon="mdi-help"
                    ></v-icon>
                  </v-btn>
                </v-col>
              </v-row>
            </v-form>
          </v-sheet>
        </v-col>
      </v-row>
    </v-container>
  </template>
  <script>
    export default {
      name: 'Main_form',
      props:{ userName: String },
      data: () => ({
        pwShow: false,
        password: '',
        userNm: ''
      }),
      methods: {
        async login(){
          this.axios.get(
            `http://localhost:3000/api/login?username=${this.userNm}&pw=${this.password}`
          )
          .then((res) =>{
            console.log(res.data.RETURN.CNT)
          })
          .catch((err)=>{
            console.log(err)
          })
        }
      }
    }
  </script>
  