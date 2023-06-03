<template>
  <v-container>
    <ResultMsg
      :isSnackbar="snackBar.isSnackbar"
      :snackbarText="snackBar.snackbarText"
      :status="snackBar.snackStatus"
      v-on:closeMsgResult="closeMsgResult"
    />
    <SysLoader 
      :isLoader="isLoader" 
    />
    <v-row>
      <v-col cols="3">
      </v-col>
      <v-col cols="7">
        <v-img
          class="mt-10"
          cover
          src="../assets/lock.png"
        ></v-img>
      </v-col>
      <v-col cols="2">
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
                <v-btn type="" block class="mt-2" color="">
                  ?
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
import ResultMsg from "../components/ResultMsg.vue";
import SysLoader from "../components/SysLoader.vue";

  export default {
    name: 'Login_form',
    props:{ userName: String },
    components: {
      SysLoader,
      ResultMsg
    },

    data: () => ({
      pwShow: false,
      password: '',
      userNm: '',
      snackBar: {
        isSnackbar: false,
        snackStatus: "",
        snackbarText: "",
        vertical: true,
      },
      isLoader: false
    }),
    mounted(){
      if(this.userName) {
        this.snackBar.snackStatus = ""
        this.snackBar.snackbarText = "See you soon " + this.userName + "!"
        this.snackBar.isSnackbar = true
      }
    },
    methods: {
      async login(){
        this.isLoader = true
        await this.axios.get(
          `http://localhost:3000/api/login?username=${this.userNm}&pw=${this.password}`
        )
        .then((res) =>{
          if(res.data.RETURN.CNT == 1){
            this.$emit('logStsChange', this.userNm)
          }
          else {
            this.snackBar.snackStatus = "E"
            this.snackBar.snackbarText = "Incorrect Information"
            this.snackBar.isSnackbar = true
          }
          this.isLoader = false
        })
        .catch((err)=>{
          console.log(err)
          this.isLoader = false
        })
        
      },
      closeMsgResult(){
        this.snackBar.snackStatus = ""
        this.snackBar.snackbarText = ""
        this.snackBar.isSnackbar = false
      }
    }
  }
</script>
