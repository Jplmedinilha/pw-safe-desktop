<template>
  <div class="text-center">
    <v-row justify="center">
    <v-dialog 
      v-model="localisAlarm"
      max-width="490"
      persistent
    >
      <v-card
      >
        <v-card-title :class="(compcritical_flg == 'Y') ? 'red' : 'primary'">
          <div class="pt-4"><label style="white-space: pre-line" id="text">  {{item.systemName}} </label></div>
        </v-card-title>
        <v-card-text>
          <v-row class="pt-4">
            <v-col
              cols="12"
              sm="12"
            >
              <v-text-field
                label="Group"
                v-model="compgroupName"
                disabled
                outlined
                hide-details
              ></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col
              cols="12"
              sm="12"
            >
              <v-text-field
                label="Username"
                v-model="compcred_user"
                disabled
                outlined
                hide-details
              ></v-text-field>
            </v-col>
          </v-row>
          <v-row class="pt-4">
            <v-col
              cols="12"
              sm="12"
            >
              <v-text-field
                label="Secret"
                v-model="secret"
                class="input-group--focused"               
                outlined
                hide-details
              ></v-text-field>
            </v-col>
          </v-row>
          <!-- <v-row class="pt-4">
            <v-col
              cols="12"
              sm="12"
            >
              <v-text-field
                label="Password"
                v-model="compcred_pw"
                :type="show ? 'text' : 'password'"
                class="input-group--focused"               
                outlined
                disabled
                hide-details
              ></v-text-field>
            </v-col>
          </v-row> -->
        </v-card-text>
            
        <v-card-actions>
          <v-row>
              <v-btn small color="blue" @click="descryptPW">Decrypt</v-btn>
            </v-row>
          <v-btn
            text
            class="red"
            small
            @click="deleteCredential"
          >
            Delete
          </v-btn>
          <v-spacer></v-spacer> 
          <v-btn
            text
            small
            @click="closeMSG"
          >
            close
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
  <ResultMsg
      :isSnackbar="snackBar.isSnackbar"
      :snackbarText="snackBar.snackbarText"
      :status="snackBar.snackStatus"
      v-on:closeMsgResult="closeMsgResult"
    />
  <ConfirmDeleteMsg
        :isSnackbar="ConfirmMsg.isConf"
        :confirmText="ConfirmMsg.confirmText"
        :confirmStatus="ConfirmMsg.confirmStatus"
        v-on:onDeleteCanceled="onConfirmCancel"
        v-on:onDeleteConfirmed="onDeleteGroupConfirm"
      />
  </div>
  
</template>
<script>
import ResultMsg from "../components/ResultMsg.vue";
import ConfirmDeleteMsg from "../components/ConfirmDeleteMsg.vue";

export default {
    props:{ isAlarm: Boolean, item: Object },
    components: {
      ResultMsg,
      ConfirmDeleteMsg
    },
    data: () => ({
      display: false,
      secret:'',
      encrypted: '',
      snackBar: {
        isSnackbar: false,
        snackStatus: "",
        snackbarText: "",
        vertical: true,
      },
      ConfirmMsg: {
          isConf: false,
          confirmText: "",
          confirmStatus: ''
        },
    }),
    methods:{
      closeMSG(){
          this.secret = ""
          this.$emit('closeMsgResult')
      },
      displayPw(){
        this.display = !this.display
      },
      decryptMethod(){
        this.$CryptoJS.AES.decrypt(this.compcred_pw, this.secret).toString(this.$CryptoJS.enc.Utf8)
      },
      closeMsgResult(){
        this.snackBar.snackStatus = ""
        this.snackBar.snackbarText = ""
        this.snackBar.isSnackbar = false
      },
      descryptPW(){
        if(!this.secret){
          this.snackBar.snackStatus = "E"
          this.snackBar.snackbarText = "Missing information"
          this.snackBar.isSnackbar = true
        } else {
          let decrypt = this.$CryptoJS.AES.decrypt(this.compcred_pw, this.secret).toString(this.$CryptoJS.enc.Utf8)
          if(decrypt == '') {
            this.snackBar.snackStatus = "E"
            this.snackBar.snackbarText = "Wrong secret"
            this.snackBar.isSnackbar = true
          } else {
            this.snackBar.snackStatus = ""
            this.snackBar.snackbarText = decrypt
            this.snackBar.isSnackbar = true
          }
          
        }
      },
      deleteCredential(){
        this.ConfirmMsg.confirmText = `Are you sure you want to delete ${this.item.systemName}?`
        this.ConfirmMsg.confirmStatus = "E"
        this.ConfirmMsg.isConf = true
      },
      onConfirmCancel(){
        this.ConfirmMsg.confirmText = ""
        this.ConfirmMsg.confirmStatus = ""
        this.ConfirmMsg.isConf = false

      },
      async onDeleteGroupConfirm(){
        this.$emit('deleteCredential', this.compgroupName, this.item.systemName, this.compcred_user)//deleteCredential(groupName, systemName, credUser)
        this.closeMSG();
      },
    },
    computed:{
      localisAlarm:{
        get(){
          return this.isAlarm
        }
      },
      localsnackbarText:{
        get(){
          return this.snackbarText
        }
      },
      localstatus:{
        get(){
          return this.status
        }
      },
      compcred_pw:{
        get(){
          return this.item.cred_pw
        }
      },
      compcred_user:{
        get(){
          return this.item.cred_user
        }
      },
      compgroupName:{
        get(){
          return this.item.groupName
        }
      },
      compcritical_flg:{
        get(){
          return this.item.critical_flg
        }
      },
      show(){
        if(this.compcritical_flg == 'N') {
          return true
        }
        else return (this.display)
      },
      
    },
}
</script>
<style scoped>
#text {
  font-size: 20px !important;
}
</style>