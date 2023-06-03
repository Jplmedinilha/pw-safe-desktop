<template>
  <div class="text-center">
    <v-row justify="center">
    <v-dialog 
      v-model="localIsSnackbar"
      persistent
    >
      <v-card
        :class="(confirmStatus=='E') ? 'error' : 'blue'"
      >
        <v-card-text :class="(confirmStatus=='E') ? 'text-h5 red' : 'text-h5  blue'">
          <div class="pt-4"><label style="white-space: pre-line" id="text" class="font-weight-thin white--text">  {{ confirmText }} </label></div>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer> 
          <v-btn
            color="white"
            text
            @click="onConfirm"
          >
            Yes
          </v-btn>
          <v-btn
            color="white"
            text
            @click="closeMSG"
          >
            No
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
  </div>
</template>
<script>

export default {
    props:{ isSnackbar: Boolean, confirmText: String, confirmStatus: String },
    methods:{
        closeMSG(){
            this.$emit('onDeleteCanceled')
        },
        onConfirm(){
            this.$emit('onDeleteConfirmed')
        }
    },
    computed:{
      localIsSnackbar:{
        get(){
          return this.isSnackbar
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