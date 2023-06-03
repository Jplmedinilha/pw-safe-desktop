<template>
  <div class="text-center">
    <v-row justify="center">
    <v-dialog 
      v-model="localIsSnackbar"
      persistent
      max-width="490"
    >
      <v-card
        :class="(status=='E') ? 'error' : 'blue'"
      >
        <v-card-text :class="(status=='E') ? 'text-h5 red' : 'text-h5  blue'">
          <div class="pt-4"><label style="white-space: pre-line" id="text" class="font-weight-thin white--text">  {{ snackbarText }} </label></div>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer> 
          <v-btn
            color="white"
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
    props:{ isSnackbar: Boolean, snackbarText: String, status: String },
    methods:{
        closeMSG(){
            this.$emit('closeMsgResult')
        }
    },
    computed:{
      localIsSnackbar:{
        get(){
          return this.isSnackbar
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
      }
    }
}
</script>
<style scoped>
#text {
  font-size: 20px !important;
}
</style>