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
                label="Password"
                v-model="compcred_pw"
                :type="show ? 'text' : 'password'"
                class="input-group--focused"               
                outlined
                disabled
                hide-details
              ></v-text-field>
            </v-col>
            <!-- <v-col
              cols="2"
              sm="2"
            >
              <v-btn small ><v-icon :icon="show ? 'mdi-eye' : 'mdi-eye-off'"></v-icon></v-btn>
            </v-col> -->
          </v-row>
        </v-card-text>
        <v-card-actions>
          <v-btn
            v-if="compcritical_flg == 'Y'"
            @click="displayPw"
            :append-icon="true ? 'mdi-eye' : 'mdi-eye-off'"
          >
            Display
            <v-icon
              end
              icon="mdi-checkbox-marked-circle"
            ></v-icon> 
          </v-btn>
          <v-spacer></v-spacer> 
          <v-btn
            text
            @click="closeMSG"
          >
            close
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
  </div>
</template>
<script>
export default {
    props:{ isAlarm: Boolean, item: Object },
    data: () => ({
      display: false
    }),
    methods:{
      closeMSG(){
          this.$emit('closeMsgResult')
      },
      displayPw(){
        this.display = !this.display
      }
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